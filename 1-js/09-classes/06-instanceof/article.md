# Class-ի ստուգում․ «instanceof»

`instanceof` օպերատորը թույլ է տալիս ստուգել՝ արդյո՞ք օբյեկտը պատկանում է որևէ class-ին: Այն նաև հաշվի է առնում ժառանգությունը:

Նման ստուգումը շատ դեպքերում կարող է անհրաժեշտ լինել: Օրինակ՝ այն կարող է օգտագործվել *պոլիմորֆ* (polymorphic) ֆունկցիա կառուցելու համար, որը տարբեր ձևերով է վերաբերվում արգումենտներին՝ կախված դրանց տեսակից:

## Instanceof օպերատորը [#ref-instanceof]

Շարահյուսությունը հետևյալն է.
```js
obj instanceof Class
```

Այն վերադարձնում է `true`, եթե `obj`-ն պատկանում է `Class`-ին կամ նրանից ժառանգած class-ին:

Օրինակ՝

```js run
class Rabbit {}
let rabbit = new Rabbit();

// արդյո՞ք դա Rabbit class-ի օբյեկտ է
*!*
alert( rabbit instanceof Rabbit ); // true
*/!*
```

Այն նաև աշխատում է կոնստրուկտոր ֆունկցիաների հետ.

```js run
*!*
// instead of class
function Rabbit() {}
*/!*

alert( new Rabbit() instanceof Rabbit ); // true
```

...Նաև ներկառուցված class-ների հետ, ինչպիսին է `Array`-ը․

```js run
let arr = [1, 2, 3];
alert( arr instanceof Array ); // true
alert( arr instanceof Object ); // true
```

Նկատի ունեցեք, որ `arr`-ը նույնպես պատկանում է `Object` class-ին: Դա պայմանավորված է նրանով, որ `Array`-ը նախատիպով ժառանգում է `Object`-ից:

Սովորաբար, ստուգելու համար `instanceof`-ն ուսումնասիրում է նախատիպային շղթան: Մենք կարող ենք նաև հատուկ տրամաբանություն սահմանել `Symbol.hasInstance` ստատիկ մեթոդում:

`obj instanceof Class`-ի ալգորիթմն աշխատում է մոտավորապես հետևյալ կերպ.

1. Եթե կա `Symbol.hasInstance` ստատիկ մեթոդ, ապա պարզապես կանչել այն՝ `Class[Symbol.hasInstance](obj)`: Այն պետք է վերադարձնի կամ `true` կամ `false`, վերջ: Ահա, թե ինչպես կարող ենք կարգավորել `instanceof`-ի պահելաձևը:

    Օրինակ՝

    ```js run
    // ստեղծենք instanceOf-ի օրինակ, որը ենթադրում է, որ
    // canEat հատկություն ունեցող ցանկացած բան animal է
    class Animal {
      static [Symbol.hasInstance](obj) {
        if (obj.canEat) return true;
      }
    }

    let obj = { canEat: true };

    alert(obj instanceof Animal); // true՝ Animal[Symbol.hasInstance](obj)-ն է կանչվել
    ```

2. Class-ների մեծ մասը չունի `Symbol.hasInstance`: Այդ դեպքում օգտագործվում է ստանդարտ տրամաբանություն. `obj instanceOf Class`-ը ստուգում է՝ արդյո՞ք `Class.prototype`-ը հավասար է `obj`-ի նախատիպային շղթայում առկա նախատիպերից որևէ մեկին:

    Այլ կերպ ասած՝ համեմատում է մեկը մյուսի հետևից.
    ```js
    obj.__proto__ === Class.prototype?
    obj.__proto__.__proto__ === Class.prototype?
    obj.__proto__.__proto__.__proto__ === Class.prototype?
    ...
    // Եթե որևէ համեմատություն true է, ապա վերադարձնում է true
    // հակառակ դեպքում, երբ հասնում է շղթայի վերջին, վերադարձնում է false
    ```

    Վերոնշյալ օրինակում՝ `rabbit.__proto__ === Rabbit.prototype`, հետևաբար այն անմիջապես վերադարձնում է պատասխանը:

    Ժառանգության դեպքում համընկնումը կլինի երկրորդ քայլում.

    ```js run
    class Animal {}
    class Rabbit extends Animal {}

    let rabbit = new Rabbit();
    *!*
    alert(rabbit instanceof Animal); // true
    */!*

    // rabbit.__proto__ === Animal.prototype (չկա համընկնում)
    *!*
    // rabbit.__proto__.__proto__ === Animal.prototype (համընկավ)
    */!*
    ```

Ահա պատկերազարդումը, թե ինչ է `rabbit instanceof Animal`-ը համեմատվում `Animal.prototype`-ի հետ.

![](instanceof.svg)

Ի դեպ, կա նաև մեթոդ՝ [objA.isPrototypeOf(objB)](mdn:js/object/isPrototypeOf), որը վերադարձնում է `true`, եթե `objA`-ն ինչ-որ տեղ `objB`-ի նախատիպերի շղթայում է: Այսպիսով, «obj instanceof Class»-ի թեստը կարող է վերաձեւակերպվել որպես «Class.prototype.isPrototypeOf(obj)»:

Ծիծաղելի է, բայց `Class` կոնստրուկտորն ինքնին չի մասնակցում ստուգմանը: Միայն նախատիպերի շղթան է կարևոր և `Class.prototype`-ը:

Դա կարող է հանգեցնել հետաքրքիր հետևանքների, երբ օբյեկտի ստեղծումից հետո փոխվում է `prototype`-ի հատկությունը:

Ինչպես այստեղ․

```js run
function Rabbit() {}
let rabbit = new Rabbit();

// փոփոխվեց նախատիպը
Rabbit.prototype = {};

// ...այլևս rabbit չէ
*!*
alert( rabbit instanceof Rabbit ); // false
*/!*
```

## Բոնուս․ Object.prototype.toString-ը տիպերի համար

Մենք արդեն գիտենք, որ պարզ օբյեկտները վերածվում են տողի (string), որպես՝ `[object Object]`․

```js run
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // նույնը
```

Դա `toString`-ի իրագործումն է: Բայց կա մի թաքնված հատկություն, որը `toString`-ն իրականում դրանից շատ ավելի զորեղ է դարձնում: Մենք կարող ենք օգտագործել այն որպես ընդլայնված `typeof` և `instanceof`-ի համար այլընտրանք:

Տարօրինա՞կ է հնչում։ Իսկապես։ Եկեք փորձարկենք.

Ըստ [specification](https://tc39.github.io/ecma262/#sec-object.prototype.tostring)-ի, ներկառուցված `toString`-ը կարող է հանվել օբյեկտից և գործարկվել ցանկացած այլ արժեքի համատեքստում: Եվ դրա արդյունքը կախված է այդ արժեքից։

- Թվերի դեպքում, այն կլինի `[object Number]`
- Բուլյանի դեպքում, այն կլինի `[object Boolean]`
- Երբ `null` է՝ `[object Null]`
- Երբ `undefined` է՝ `[object Undefined]`
- Զանգվածների համար՝ `[object Array]`
- ...և այլն (կարգավորելի)։

Եկեք ցուցադրենք․

```js run
// հարմարության համար պատճենեք toString մեթոդը փոփոխականի մեջ
let objectToString = Object.prototype.toString;

// ի՞նչ տեսակ է սա
let arr = [];

alert( objectToString.call(arr) ); // [object *!*Array*/!*]
```

Այստեղ մենք օգտագործեցինք [call](mdn:js/function/call)-ը՝ ինչպես նկարագրված էր այս գլխում՝ [](info:call-apply-decorators), որպեսզի գործարկենք `objectToString` ֆունկցիան `this=arr` կոնտեքստում։

Ներքին կարգով, `toString`-ի ալգորիթմը ուսումնասիրում է `this`-ը և վերադարձնում համապատասխան արդյունքը: Լրացուցիչ օրինակներ.

```js run
let s = Object.prototype.toString;

alert( s.call(123) ); // [object Number]
alert( s.call(null) ); // [object Null]
alert( s.call(alert) ); // [object Function]
```

### Symbol.toStringTag

Օբյեկտից կախված, `toString`-ի վարքագիծը կարող է հարմարեցվել՝ օգտագործելով `Symbol.toStringTag` հատուկ օբյեկտի հատկությունը:

Օրինակ․

```js run
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]
```

Շրջակա միջավայրին հատուկ օբյեկտների մեծ մասի համար կա այդպիսի հատկություն։ Ահա որոշ բրաուզերի հատուկ օրինակներ.

```js run
// toStringTag-ը միջավայրին հատուկ օբյեկտի and class-ի համար․
alert( window[Symbol.toStringTag]); // Window
alert( XMLHttpRequest.prototype[Symbol.toStringTag] ); // XMLHttpRequest

alert( {}.toString.call(window) ); // [object Window]
alert( {}.toString.call(new XMLHttpRequest()) ); // [object XMLHttpRequest]
```

Ինչպես տեսնում եք, արդյունքը հենց `Symbol.toStringTag` է (եթե գոյություն ունի)՝ փաթաթված `[object ...]`-ում:

Վերջում մենք ունենք «typeof-ը ստերոիդների համար», որը ոչ միայն աշխատում է պարզ (primitive) տվյալների տեսակների, այլև ներկառուցված օբյեկտների համար և նույնիսկ կարող է հարմարեցվել, կարգավորվել:

Կարող ենք նաև ներկառուցված օբյեկտների համար օգտագործել `{}.toString.call`-ը՝ `instanceof`-ի փոխարեն, երբ մեզ անհրաժեշտ է ստանալ տեսակը որպես տող, այլ ոչ թե պարզապես ստուգելու համար:

## Ամփոփում

Եկեք ամփոփենք տիպերի ստուգման մեթոդները, որոնք մեզ հայտնի են.

|               | works for   |  returns      |
|---------------|-------------|---------------|
| `typeof`      | primitives  |  string       |
| `{}.toString` | primitives, built-in objects, objects with `Symbol.toStringTag`   |       string |
| `instanceof`  | objects     |  true/false   |

Ինչպես տեսնում ենք, `{}.toString`-ը տեխնիկապես «ավելի առաջադեմ» `typeof` է:

Իսկ `instanceof` օպերատորը իսկապես փայլում է, երբ աշխատում ենք class-ի հիերարխիայի հետ և ցանկանում ենք ստուգել class-ի առկայությունը՝ հաշվի առնելով ժառանգականությունը:
