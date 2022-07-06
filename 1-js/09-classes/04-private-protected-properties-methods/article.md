
# Մասնավոր և պաշտպանված հատկություններ ու մեթոդներ

Օբյեկտ կողմնորոշված ծրագրավորման ամենակարևոր սկզբունքներից մեկն է՝ ներքին ինտերֆեյսի սահմանազատումն արտաքինից:

Դա «պարտադիր» պրակտիկա է «բարև աշխարհ» հավելվածից ավելի բարդ բան մշակելու համար:

Սա հասկանալու համար եկեք կտրվենք ծրագրավորումից և մեր հայացքն ուղղենք դեպի իրական աշխարհ:

Սովորաբար, սարքերը, որոնք մենք օգտագործում ենք, բավականին բարդ են: Բայց ներքին ինտերֆեյսը արտաքինից սահմանազատելը թույլ է տալիս դրանք օգտագործել առանց խնդիրների։

## Իրական կյանքի օրինակ

Օրինակ՝ սուրճի մեքենան։ Դրսից պարզ է՝ կոճակ, էկրան, մի քանի անցք... Եվ, անկասկած, արդյունքը՝ հիանալի սուրճ: :)

![](coffee.jpg)

Բայց ներսում... (նկար վերանորոգման ձեռնարկից)

![](coffee-inside.jpg)

Շատ մանրամասներ։ Բայց մենք, առանց դրա մասին որևէ բան իմանալու, կարող ենք օգտագործել այն:

Սուրճի մեքենաները բավականին հուսալի են, այնպես չէ՞: Մենք կարող ենք օգտագործել դրանցից մեկը տարիներ շարունակ, և միայն այն դեպքում, երբ ինչ-որ բան չի աշխատում, տանել այն վերանորոգման:

Սուրճի մեքենայի հուսալիության և պարզության գաղտնիքը. բոլոր դետալները լավ կարգաբերված են և *թաքցրած* են ներսում:

Եթե սուրճի մեքենայի վրայից հանենք պաշտպանիչ ծածկույթը, ապա դրա օգտագործումը կլինի շատ ավելի բարդ (որտե՞ղ սեղմել) և վտանգավոր (կարող է էլեկտրահարել):

Ինչպես կնկատենք, ծրագրավորման մեջ օբյեկտները նման են սուրճի մեքենաներին:

Բայց ներքին դետալները թաքցնելու համար մենք կօգտագործենք ոչ թե պաշտպանիչ ծածկույթ, այլ լեզվի հատուկ շարահյուսություն և պայմանականություններ:

## Ներքին և արտաքին ինտերֆեյս

Օբյեկտ-կողմնորոշված ծրագրավորման մեջ հատկությունները և մեթոդները բաժանվում են երկու խմբի.

- *Ներքին ինտերֆեյս*՝ մեթոդներ և հատկություններ, որոնք հասանելի են class-ի այլ մեթոդներից, բայց ոչ դրսից:
- *Արտաքին ինտերֆեյս*՝ մեթոդներ և հատկություններ, որոնք հասանելի են նաև class-ից դուրս։

Եթե մենք շարունակենք անալոգիան սուրճի մեքենայի հետ, ապա այն, ինչ թաքնված է ներսում՝ կաթսայի խողովակ, ջեռուցման տարր և այլն, նրա ներքին ինտերֆեյսն է:

Օբյեկտի աշխատանքի համար օգտագործվում է ներքին ինտերֆեյսը, դրա դետալներն օգտագործում են միմյանց: Օրինակ՝ ջեռուցման տարրին միացված է կաթսայի խողովակը:

Բայց դրսից սուրճի մեքենան փակված է պաշտպանիչ ծածկույթով, որպեսզի ոչ ոք չդիպչի դրանց։ Դետալները թաքնված են և անհասանելի։ Մենք կարող ենք օգտագործել մեքենայի հնարավորությունները արտաքին ինտերֆեյսի միջոցով:

Այսպիսով, օբյեկտն օգտագործելու համար մեզ անհրաժեշտ է իմանալ միայն դրա արտաքին ինտերֆեյսը: Մենք կարող ենք ամբողջովին անտեղյակ լինել, թե ինչպես է այն աշխատում ներսում և դա հիանալի է:

Դա ընդհանուր ներածություն էր։

JavaScript-ում կա օբյեկտների դաշտերի երկու տեսակ (հատկություններ և մեթոդներ).

- Հանրային (public)․ հասանելի է ցանկացած վայրից։ Սրանք ներառում են արտաքին ինտերֆեյսը: Մինչ այս մենք օգտագործում էինք միայն հանրային հատկություններն ու մեթոդները։
- Մասնավոր (private)․ հասանելի է միայն class-ի ներսում։ Սրանք ներքին ինտերֆեյսի համար են:

Շատ այլ լեզուներում կան նաև «պաշտպանված» (protected) դաշտեր. հասանելի են միայն class-ի ներսից և այն ընդլայնողներից (ինչպես մասնավորը, բայց դրան գումարած նաև հասանելիությունը ժառանգական class-ներից): Դրանք նաև օգտակար են ներքին ինտերֆեյսի համար: Դրանք ինչ-որ առումով ավելի տարածված են, քան մասնավորները, քանի որ մեզ սովորաբար անհրաժեշտ է լինում, որ ժառանգական class-ները հասանելիություն ունենան դրանց:

Պաշտպանված դաշտերը չեն ներդրվում JavaScript-ում լեզվի մակարդակով, բայց գործնականում դրանք շատ հարմար են, ուստի նմանակվում են։

Այժմ մենք կպատրաստենք սուրճի մեքենա JavaScript-ով այս բոլոր տեսակի հատկություններով: Սուրճի մեքենան ունի շատ դետալներ, մենք չենք մոդելավորի դրանք պարզ մնալու համար (թեև կարող էինք):

## «WaterAmount»-ի պաշտպանություն

Եկեք նախ պատրաստենք սուրճի մեքենայի պարզ class.

```js run
class CoffeeMachine {
  waterAmount = 0; // ջրի տարողությունը

  constructor(power) {
    this.power = power;
    alert( `Ստեղծվել է սուրճի մեքենա, հզորություն՝ ${power}` );
  }

}

// ստեղծել սուրճի մեքենան
let coffeeMachine = new CoffeeMachine(100);

// ավելացնել ջուր
coffeeMachine.waterAmount = 200;
```

Այս պահին `waterAmount` և `power` հատկությունները հրապարակային են: Մենք դրսից հեշտությամբ կարող ենք ստանալ և սահմանել դրանց համար ցանկացած արժեք:

Եկեք փոխենք `waterAmount` հատկությունը պաշտպանվածի, որպեսզի ավելի շատ վերահսկողություն ունենանք դրա նկատմամբ: Օրինակ՝ մենք չենք ուզում, որ որևէ մեկը այն սահմանի զրոյից ցածր:

**Պաշտպանված (protected) հատկությունները սովորաբար նշվում են `_` ընդգծված նախածանցով։**

Դա չի կիրառվում լեզվի մակարդակով, բայց ծրագրավորողների միջև հայտնի կոնվենցիա կա, որ նման հատկություններն ու մեթոդները չպետք է հասանելի լինեն դրսից:

Այսպիսով, մեր հատկությունը կկոչվի `_waterAmount`:

```js run
class CoffeeMachine {
  _waterAmount = 0;

  set waterAmount(value) {
    if (value < 0) {
      value = 0;
    }
    this._waterAmount = value;
  }

  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) {
    this._power = power;
  }

}

// ստեղծել սուրճի մեքենան
let coffeeMachine = new CoffeeMachine(100);

// ավելացնել ջուր
coffeeMachine.waterAmount = -10; // _waterAmount կլինի 0, ոչ՝ -10
```

Այժմ հասանելիությունը վերահսկվում է, ուստի ջրի տարողությունը զրոյից ցածր սահմանելն անհնար է դառնում:

## Միայն-ընթեռնելի (read-only) «հզորություն»

Եկեք `power` հատկությունը դարձնենք միայն-ընթեռնելի: Երբեմն պատահում է, որ հատկությունը պետք է սահմանվի միայն ստեղծման պահին, այնուհետև երբեք չփոփոխվի:

Դա հենց այդպես է սուրճի մեքենայի դեպքում. հզորությունը երբեք չի փոխվում:

Դա անելու համար մեզ միայն անհրաժեշտ է ստեղծել գեթթեր, բայց ոչ սեթթեր.

```js run
class CoffeeMachine {
  // ...

  constructor(power) {
    this._power = power;
  }

  get power() {
    return this._power;
  }

}

// ստեղծել սուրճի մեքենան
let coffeeMachine = new CoffeeMachine(100);

alert(`Հզորությունը՝ ${coffeeMachine.power}W`); // Հզորությունը՝ 100W

coffeeMachine.power = 25; // Error (no setter)
```

````smart header="Getter/setter ֆունկցիաներ"
Այստեղ մենք օգտագործեցինք getter/setter շարահյուսությունը։

But most of the time `get.../set...` functions are preferred, like this:
Բայց շատ ժամանակ գերադասելի են `get.../set...` ֆունկցիաներ, այսպես.

```js
class CoffeeMachine {
  _waterAmount = 0;

  *!*setWaterAmount(value)*/!* {
    if (value < 0) value = 0;
    this._waterAmount = value;
  }

  *!*getWaterAmount()*/!* {
    return this._waterAmount;
  }
}

new CoffeeMachine().setWaterAmount(100);
```

Մի փոքր ավելի երկար է թվում, բայց ֆունկցիաներն ավելի ճկուն են: Նրանք կարող են ընդունել բազմաթիվ արգումենտներ (նույնիսկ, եթե դրանք մեզ հիմա պետք չեն):

Մյուս կողմից, get/set շարահյուսությունն ավելի կարճ է, ուստի, ի վերջո, խիստ կանոն չկա, որոշումը ձերն է:
````

```smart header="Պաշտպանված դաշտերը ժառանգվում են"
Եթե մենք ժառանգում ենք՝ `class MegaMachine extends CoffeeMachine`, ապա ոչինչ չի խանգարում մեզ մուտք գործել `this._waterAmount` կամ `this._power` նոր class-ի մեթոդներից:

Այսպիսով, պաշտպանված դաշտերը բնականաբար ժառանգվում են, ի տարբերություն մասնավորների, որոնք կտեսնենք ստորև։
```

## Մասնավոր «#waterLimit»

[recent browser=none]

Կա ավարտված JavaScript առաջարկ, գրեթե ստանդարտում, որն ապահովում է լեզվի մակարդակով աջակցություն մասնավոր հատկությունների և մեթոդների համար:

Մասնավորները պետք է սկսվեն `#`-ով: Դրանք հասանելի են միայն class-ի ներսում:

Օրինակ՝ ահա մասնավոր `#waterLimit` հատկությունը և ջրի ստուգման մասնավոր մեթոդը՝ `#fixWaterAmount`․

```js run
class CoffeeMachine {
*!*
  #waterLimit = 200;
*/!*

*!*
  #fixWaterAmount(value) {
    if (value < 0) return 0;
    if (value > this.#waterLimit) return this.#waterLimit;
  }
*/!*

  setWaterAmount(value) {
    this.#waterLimit = this.#fixWaterAmount(value);
  }

}

let coffeeMachine = new CoffeeMachine();

*!*
// class-ից դուրս հասանելիություն չկա դեպի մասնավորներ 
coffeeMachine.#fixWaterAmount(123); // Error
coffeeMachine.#waterLimit = 1000; // Error
*/!*
```

Լեզվի մակարդակում `#`-ը հատուկ նիշ է, որը մասնավոր է դարձնում դաշտը: Մենք չենք կարող հասանելիություն ունենալ դրան դրսից կամ ժառանգական class-ներից:

Մասնավոր դաշտերը չեն հակասում հանրայինի հետ։ Մենք կարող ենք միաժամանակ ունենալ և՛ մասնավոր `#waterAmount`, և՛ հանրային `waterAmount` դաշտեր:

Օրինակ՝ եկեք `waterAmount`-ը դարձնենք `#waterAmount`-ի մուտքային (accessor).

```js run
class CoffeeMachine {

  #waterAmount = 0;

  get waterAmount() {
    return this.#waterAmount;
  }

  set waterAmount(value) {
    if (value < 0) value = 0;
    this.#waterAmount = value;
  }
}

let machine = new CoffeeMachine();

machine.waterAmount = 100;
alert(machine.#waterAmount); // Error
```

Ի տարբերություն պաշտպանվածների, մասնավոր դաշտերը պարտադրվում են հենց լեզվով: Դա լավ բան է:

Բայց, եթե մենք ժառանգենք `CoffeeMachine`-ից, ապա մենք անմիջական մուտք չենք ունենա `#waterAmount`-ին: Մենք պետք է ապավինենք `waterAmount` գեթթերին/սեթթերին.

```js
class MegaCoffeeMachine extends CoffeeMachine {
  method() {
*!*
    alert( this.#waterAmount ); // Error: can only access from CoffeeMachine
*/!*
  }
}
```

Շատ սցենարներում նման սահմանափակումը չափազանց խիստ է: Եթե մենք ընդլայնենք `CoffeeMachine`-ը, մենք կարող ենք օրինական պատճառներ ունենալ դրա ներքին բաղադրիչներին հասանելիություն ունենալու համար: Այդ իսկ պատճառով պաշտպանված դաշտերն ավելի հաճախ են օգտագործվում, թեև դրանք չեն ապահովվում լեզվի շարահյուսությամբ:

````warn header="Մասնավոր դաշտերը հասանելի չեն որպես this[name]"
Մասնավոր դաշտերը հատուկ են:

Ինչպես գիտենք, սովորաբար մենք կարող ենք մուտք գործել դաշտեր՝ օգտագործելով `this[name]`․

```js
class User {
  ...
  sayHi() {
    let fieldName = "name";
    alert(`Hello, ${*!*this[fieldName]*/!*}`);
  }
}
```

Մասնավոր դաշտերի դեպքում դա անհնար է. `this['#name']`-ը չի աշխատում: Դա շարահյուսական սահմանափակում է՝ գաղտնիությունն ապահովելու համար:
````

## Ամփոփում

In terms of OOP, delimiting of the internal interface from the external one is called [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)).

It gives the following benefits:

Protection for users, so that they don't shoot themselves in the foot
: Imagine, there's a team of developers using a coffee machine. It was made by the "Best CoffeeMachine" company, and works fine, but a protective cover was removed. So the internal interface is exposed.

    All developers are civilized -- they use the coffee machine as intended. But one of them, John, decided that he's the smartest one, and made some tweaks in the coffee machine internals. So the coffee machine failed two days later.

    That's surely not John's fault, but rather the person who removed the protective cover and let John do his manipulations.

    The same in programming. If a user of a class will change things not intended to be changed from the outside -- the consequences are unpredictable.

Supportable
: The situation in programming is more complex than with a real-life coffee machine, because we don't just buy it once. The code constantly undergoes development and improvement.

    **If we strictly delimit the internal interface, then the developer of the class can freely change its internal properties and methods, even without informing the users.**

    If you're a developer of such class, it's great to know that private methods can be safely renamed, their parameters can be changed, and even removed, because no external code depends on them.

    For users, when a new version comes out, it may be a total overhaul internally, but still simple to upgrade if the external interface is the same.

Hiding complexity
: People adore using things that are simple. At least from outside. What's inside is a different thing.

    Programmers are not an exception.

    **It's always convenient when implementation details are hidden, and a simple, well-documented external interface is available.**

To hide an internal interface we use either protected or private properties:

- Protected fields start with `_`. That's a well-known convention, not enforced at the language level. Programmers should only access a field starting with `_` from its class and classes inheriting from it.
- Private fields start with `#`. JavaScript makes sure we can only access those from inside the class.

Right now, private fields are not well-supported among browsers, but can be polyfilled.
