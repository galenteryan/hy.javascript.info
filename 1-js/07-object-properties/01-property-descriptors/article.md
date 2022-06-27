
# Հատկության դրոշակներ և նկարագրիչներ

Ինչպես գիտենք, օբյեկտները կարող են պահել հատկություններ:

Մինչև հիմա հատկությունը պարզ «բանալի-արժեք» զույգ էր մեզ համար: Բայց օբյեկտի հատկությունն իրականում ավելի ճկուն և հզոր բան է:

Այս գլխում մենք կսովորենք լրացուցիչ կարգավորման պարամետրեր, իսկ հաջորդ գլխում կտեսնենք ինչպես դրանք անտեսանելի կերպով վերածել գեթթեր/սեթթեր ֆունկցիաների:

## Հատկության դրոշակներ

Բացի **`value`**-ից, օբյեկտի հատկություններն ունեն երեք հատուկ ատրիբուտներ (այսպես կոչված «դրոշակներ»):

- **`writable`** -- եթե `true` է, ապա արժեքը կարող է փոփոխվել, այլապես այն միայն ընթեռնելի է՝ անգրառելի:
- **`enumerable`** -- եթե `true` է, ապա թվարկվում է ցիկլերում, այլապես չի թվարկվում:
- **`configurable`** -- եթե `true` է, ապա հատկությունը կարող է ջնջվել, իսկ այդ ատրիբուտները կարող են փոփոխվել, այլապես՝ ոչ։

Մենք դեռ չենք հանդիպել դրանց, որովհետև հիմնականում դրանք չեն ցուցադրվում: Երբ մենք ստեղծում ենք հատկություն «սովորական ձևով», դրանք բոլորը `true` են։ Բայց մենք կարող ենք նաև փոփոխել դրանք ցանկացած ժամանակ:

Նախ, եկեք տեսնենք, թե ինչպես ստանալ այդ դրոշակները:

Այս մեթոդը՝ [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor), թույլ է տալիս հարցում կատարել հատկության *ամբողջական* տեղեկատվության վերաբերյալ:

Շարահյուսությունը կլինի.
```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

`obj`
: Օբյեկտ, որտեղից տեղեկատվություն ենք ստանալու։

`propertyName`
: Հատկության անվանումը:

Վերադարձված արժեքը այսպես կոչված «հատկությունների նկարագրիչ» օբյեկտ է. այն պարունակում է արժեքը և բոլոր դրոշակները:

Օրինակ․

```js run
let user = {
  name: "Մեսրոպ"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* հատկության նկարագրիչ.
{
  "value": "Մեսրոպ",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

Դրոշակները փոփոխելու համար կարող ենք օգտագործել [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)։

Շարահյուսությունը կլինի.

```js
Object.defineProperty(obj, propertyName, descriptor)
```

`obj`, `propertyName`
: Օբյեկտը և իր հատկությունը, որի համար պետք է կիրառվի նկարագրիչը:

`descriptor`
: Հատկության համար կիրառվող նկարագրիչ օբյեկտը։

Եթե հատկությունը գոյություն ունի, `defineProperty`-ն թարմացնում է դրա դրոշակները. Այլապես, այն ստեղծում է հատկությունը նշված արժեքով և դրոշակներով և այդ դեպքում, եթե դրոշակը չի նշվել, այն արժևորվում է որպես `false`։

Օրինակի համար, այստեղ ստեղծվում է `name` հատկությունը, որի բոլոր դրոշակները կեղծ արժեք ունեն․

```js run
let user = {};

*!*
Object.defineProperty(user, "name", {
  value: "Մեսրոպ"
});
*/!*

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "Մեսրոպ",
*!*
  "writable": false,
  "enumerable": false,
  "configurable": false
*/!*
}
 */
```

Համեմատեք այն «սովորական ձևով ստեղծված» վերոնշյալ `user.name`-ի հետ․ այժմ բոլոր դրոշակները կեղծ են: Եթե դա այն չէ, ինչ մենք ուզում ենք, ապա ավելի լավ է դրանք սահմանենք `descriptor`-ում՝ տալով `true` արժեք։

Այժմ եկեք տեսնենք դրոշակների ազդեցությունը օրինակով․

## Անգրառելի

Եկեք `user.name`-ը դարձնենք անգրառելի (չի կարող վերանշանակվել)՝ փոփոխելով `writable` դրոշակը․

```js run
let user = {
  name: "Մեսրոպ"
};

Object.defineProperty(user, "name", {
*!*
  writable: false
*/!*
});

*!*
user.name = "Պողոս"; // Ախալ․ հնարավոր չէ նշանակել միայն ընթեռնելի հատկությունը՝ name
*/!*
```

Այժմ ոչ ոք չի կարող փոփոխել մեր օգտատիրոջ անունը, մինչև նրանք չկիրառեն իրենց `defineProperty`-ն՝ մերը չեղարկելու համար:

```smart header="Սխալները հայտնվում են միայն խիստ ռեժիմում (strict mode)"
Ոչ խիստ ռեժիմում (non-strict mode) սխալներ չեն երևում, երբ արժևորում ենք անգրառելի և նմանատիպ հատկությունները։ Բայց գործողությունը, այնուամենայնիվ, չի հաջողվի: Դրոշակների կանոնները խախտող գործողությունները պարզապես լուռ անտեսվում են ոչ խիստ ռեժիմում:
```

Ահա նույն օրինակը, բայց հատկությունը ստեղծվում է զրոյից․

```js run
let user = { };

Object.defineProperty(user, "name", {
*!*
  value: "Մեսրոպ",
  // նոր հատկությունների համար պետք է հստակ թվարկենք, թե որն է true
  enumerable: true,
  configurable: true
*/!*
});

alert(user.name); // Մեսրոպ
user.name = "Պողոս"; // Error
```

## Անթվարկելի

Հիմա եկեք առանձին `toString` ավելացնենք `user`-ին։

Սովորաբար, օբյեկտների համար ներկառուցված `toString`-ը թվարկելի չէ, այն չի երևում `for..in`-ում: Բայց եթե մենք ավելացնենք մեր սեփական `toString`-ը, ապա լռելյայնորեն այն կհայտնվի `for..in`-ում, այսպես.

```js run
let user = {
  name: "Մեսրոպ",
  toString() {
    return this.name;
  }
};

// Լռելյայնորեն, մեր երկու հատկություններն էլ նշված են.
for (let key in user) alert(key); // name, toString
```

Եթե դա մեզ չի գոհացնում, ապա մենք կարող ենք սահմանել `enumerable:false`։ Այնուհետև այն չի հայտնվի `for..in` ցիկլում այնպես, ինչպես ներկառուցվածը.

```js run
let user = {
  name: "Մեսրոպ",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
*!*
  enumerable: false
*/!*
});

*!*
// Այժմ մեր toString-ը անհետանում է.
*/!*
for (let key in user) alert(key); // name
```

Անթվարկելի հատկությունները նաև բացառվում են `Object.keys`-ից․

```js
alert(Object.keys(user)); // name
```

## Անկարգավորելի

Անկարգավորելի դրոշակը (`configurable:false`) երբեմն նախադրված է ներկառուցված օբյեկտների և հատկությունների համար։

Չկարգավորվող հատկությունը չի կարող ջնջվել, դրա ատրիբուտները չեն կարող փոփոխվել:

Օրինակ՝ `Math.PI`-ն անգրառելի, անթվարկելի և անկարգավորելի է․

```js run
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```
Այսպիսով, ծրագրավորողը չի կարող փոխել `Math.PI`-ի արժեքը կամ վերագրել այն:

```js run
Math.PI = 3; // Սխալ, քանի որ այն ունի writable: false

// ջնջել Math.PI-ն նույնպես չի հաջողվի
```

Մենք նաև չենք կարող `Math.PI`-ն կրկին `writable` դարձնել.

```js run
// Սխալ, որովհետև՝ configurable: false
Object.defineProperty(Math, "PI", { writable: true });
```

Մենք բացարձակապես ոչինչ չենք կարող անել `Math.PI`-ի հետ:

Հատկությունը անկարգավորելի դարձնելը միակողմանի ճանապարհ է: Մենք չենք կարող այն ետ փոխել `defineProperty`-ով:

**Նկատի ունեցեք․ `configurable: false`-ը կանխում է հատկության դրոշակների փոփոխությունը և դրա ջնջումը՝ միաժամանակ թույլ տալով փոխել դրա արժեքը:**

Այստեղ `user.name`-ը անկարգավորելի է, բայց մենք դեռ կարող ենք փոփոխել այն (քանի որ այն գրառելի է)․

```js run
let user = {
  name: "Մեսրոպ"
};

Object.defineProperty(user, "name", {
  configurable: false
});

user.name = "Պողոս"; // աշխատում է լավ
delete user.name; // սխալ
```

Իսկ այտեղ `user.name`-ը դարձնում ենք «ընդմիշտ կնքված» հաստատուն, ճիշտ այնպես, ինչպես ներկառուցված `Math.PI`-ն․

```js run
let user = {
  name: "Մեսրոպ"
};

Object.defineProperty(user, "name", {
  writable: false,
  configurable: false
});

// հնարավոր չի լինի փոփոխել user.name-ը կամ դրա դրոշակները
// այս ամենը չի աշխատի.
user.name = "Պողոս";
delete user.name;
Object.defineProperty(user, "name", { value: "Պողոս" });
```

```smart header="Միակ ատրիբուտը, որի փոփոխությունը հնարավոր է․ writable true -> false"
Դրոշակները փոփոխելու հարցում մի փոքր բացառություն կա:

Անկարգավորելի հատկության համար կարող ենք փոփոխել `writable: true`-ն `false`-ով՝ այդպիսով կանխելով դրա արժեքի փոփոխությունը (պաշտպանության ևս մեկ շերտ ավելացնելու համար), սակայն ոչ հակառակը:
```

## Object.defineProperties

Կա մեթոդ՝ [Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties), որը թույլ է տալիս նշել մի քանի հատկություններ միանգամից։

Շարահյուսությունը կլինի․

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

Օրինակ․

```js
Object.defineProperties(user, {
  name: { value: "Մեսրոպ", writable: false },
  surname: { value: "Մաշտոց", writable: false },
  // ...
});
```

Այսպիսով, մենք կարող ենք միանգամից բազմաթիվ հատկություններ սահմանել:

## Object.getOwnPropertyDescriptors

Բոլոր հատկությունների նկարագրիչները միանգամից ստանալու համար կարող ենք օգտագործել [Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors) մեթոդը։

Այն `Object.defineProperties`-ի հետ միասին կարող է օգտագործվել որպես օբյեկտի կլոնավորման միջոց՝ ներառված դրոշակները.

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

Սովորաբար, երբ կլոնավորում ենք օբյեկտը, օգտագործում ենք արժևորում՝ հատկությունները պատճենելու համար, ինչպես այստեղ.

```js
for (let key in user) {
  clone[key] = user[key]
}
```

...Բայց սա չի կլոնավորում դրոշակները։ Այսպիսով, եթե ուզում ենք «ավելի լավ» կլոն, ապա նախընտրելի է `Object.defineProperties`-ը:

Մեկ այլ տարբերությունն այն է, որ `for..in`-ն անտեսում է սիմվոլիկ և անթվարկելի հատկությունները, բայց `Object.getOwnPropertyDescriptors`-ը վերադարձնում է հատկության *բոլոր* նկարագրիչները՝ ներառյալ սիմվոլիկ և անթվարկելի հատկությունները։

## Օբյեկտի գլոբալ կնքումը

Հատկության նկարագրիչները աշխատում են առանձին հատկությունների մակարդակով:

Կան նաև մեթոդներ, որոնք սահմանափակում են մուտքը դեպի *ամբողջ* օբյեկտ.

[Object.preventExtensions(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
: Արգելում է նոր հատկություններ ավելացնել օբյեկտում։

[Object.seal(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
: Արգելում է հատկությունների ավելացում/հեռացում։ Սահմանում է `configurable: false` բոլոր գոյություն ունեցող հատկությունների համար:

[Object.freeze(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
: Արգելում է հատկությունների ավելացում/հեռացում/փոփոխում։ Բոլոր գոյություն ունեցող հատկությունների համար սահմանում է `configurable: false, writable: false`:

Նրանց համար կան նաև թեստեր.

[Object.isExtensible(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)
: Վերադարձնում է `false`, եթե հատկությունների ավելացումն արգելվում է, այլապես՝ `true`:

[Object.isSealed(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)
: Վերադարձնում է `true`, եթե արգելվում է հատկությունների ավելացում/հեռացում, իսկ առկա բոլոր հատկություններն ունեն `configurable: false`։

[Object.isFrozen(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)
: Վերադարձնում է `true`, եթե հատկությունների ավելացում/հեռացում/փոփոխում արգելված է, իսկ բոլոր ընթացիկ հատկություններն ունեն `configurable: false, writable: false`:

Այս մեթոդները գործնականում հազվադեպ են օգտագործվում:
