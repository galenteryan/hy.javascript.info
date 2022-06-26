
# Հատկության դրոշակներ և նկարագրիչներ

Ինչպես գիտենք, օբյեկտները կարող են պահել հատկություներ:

Մինչև հիմա հատկությունը պարզ «բանալի-արժեք» զույգ էր մեզ համար: Բայց օբյեկտի հատկությունն իրականում ավելի ճկուն և հզոր բան է:

Այս գլխում մենք կսովորենք լրացուցիչ կարգավորման պարամետրեր, սիկ հաջորդ գլխում կտեսնենք ինչպես դրանք անտեսանելի կերպով վերածել գեթթեր/սեթթեր ֆունկցիաների:

## Հատկության դրոշակներ

Բացի **`value`**-ից, օբյեկտի հատկություններն ունեն երեք հատուկ ատրիբուտներ (այսպես կոչված «դրոշակներ»):

- **`writable`** -- եթե `true` է, ապա արժեքը կարող է փոփոխվել, այլապես այն միայն ընթեռնելի է՝ անգրառելի:
- **`enumerable`** -- եթե `true` է, ապա թվարկվում է ցիկլերում, այլապես չի թվարկվում:
- **`configurable`** -- եթե `true` է, ապա հատկությունը կարող է ջնջվել, իսկ այդ ատրիբուտները կարող են փոփոխվել, այլապես՝ ոչ։

Մենք դեռ չենք հանդիպել դրանց, որովհետև հիմնականում դրանքք չեն ցուցադրվում: Երբ մենք ստեղծում ենք հատկություն «սովորական ձևով», դրանք բոլորը `true` են։ Բայց մենք կարող ենք նաև փոփոխել դրանք ցանկացած ժամանակ:

Նախ, եկեք տեսնենք, թե ինչպես ստանալ այդ դրոշակները:

Այս մեթոդը՝ [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor), թուլ է տալիս հարցում կատարել հատկության *ամբողջական* տեղեկատվության վերաբերյալ:

Շարահյուսությունը կլինի.
```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

`obj`
: Օբյեկտ, որտեղից տեղեկատվություն ենք ստանալու։

`propertyName`
: Հատկության անվանումը:

Վերադարձված արժեքը այսպես կոչված «հատկությունների նկարագրիչ» օբյեկտ է. այն պարունակում է արժեքը և բոլոր դրոշակները:

Օրինակ՝

```js run
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* հատկության նկարագրիչ.
{
  "value": "John",
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

Եթե հատկությունը գոյություն ունի, `defineProperty`-ն թարմացնում է դրա դրոշակները. Այլապես, այն ստեղծում է հատկությունը նշված արժեքով և դրոշակներով և այդ դեպքում, եթե դրոշակը չի նշվել, այն արժեվորվում է որպես `false`։

Օրինակի համար, այստեղ ստեղծվում է `name` հատկությունը, որի բոլոր դրոշակները կեղծ արժեք ունեն․

```js run
let user = {};

*!*
Object.defineProperty(user, "name", {
  value: "John"
});
*/!*

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
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
  name: "John"
};

Object.defineProperty(user, "name", {
*!*
  writable: false
*/!*
});

*!*
user.name = "Pete"; // Error: Cannot assign to read only property 'name'
*/!*
```

Այժմ ոչ ոք չի կարող փոփոխել մեր օգտատիրոջ անունը, մինչև նրանք չկիրառեն իրենց `defineProperty`-ն՝ մերը չեղարկելու համար:

```smart header="Սխալները հայտնվում են միայն խիստ ռեժիմում (strict mode)"
Ոչ խիստ ռեժիմում (non-strict mode) սխալներ չեն երևում, երբ արժեվորում ենք անգրառելի և նմանատիպ հատկությունները։ Բայց գործողությունը, այնուամենայնիվ, չի հաջողվի: Դրոշակների կանոնները խախտող գործողությունները պարզապես լուռ անտեսվում են ոչ խիստ ռեժիմում:
```

Ահա նույն օրինակը, բայց հատկությունը ստեղծվում է զրոյից․

```js run
let user = { };

Object.defineProperty(user, "name", {
*!*
  value: "John",
  // նոր հատկությունների համար պետք է հստակ թվարկենք, թե որն է true
  enumerable: true,
  configurable: true
*/!*
});

alert(user.name); // John
user.name = "Pete"; // Error
```

## Անթվարկելի

Հիմա եկեք առանձին `toString` ավելացնենք `user`-ին։

Սովորաբար, օբյեկտների համար ներկառուցված `toString`-ը թվարկելի չէ, այն չի երևում `for..in`-ում: Բայց եթե մենք ավելացնենք մեր սեփական `toString`-ը, ապա լռելյայնորեն այն կհայտնվի `for..in`-ում, այսպես.

```js run
let user = {
  name: "John",
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
  name: "John",
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

## Non-configurable

The non-configurable flag (`configurable:false`) is sometimes preset for built-in objects and properties.

A non-configurable property can't be deleted, its attributes can't be modified.

For instance, `Math.PI` is non-writable, non-enumerable and non-configurable:

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
So, a programmer is unable to change the value of `Math.PI` or overwrite it.

```js run
Math.PI = 3; // Error, because it has writable: false

// delete Math.PI won't work either
```

We also can't change `Math.PI` to be `writable` again:

```js run
// Error, because of configurable: false
Object.defineProperty(Math, "PI", { writable: true });
```

There's absolutely nothing we can do with `Math.PI`.

Making a property non-configurable is a one-way road. We cannot change it back with `defineProperty`.

**Please note: `configurable: false` prevents changes of property flags and its deletion, while allowing to change its value.**

Here `user.name` is non-configurable, but we can still change it (as it's writable):

```js run
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  configurable: false
});

user.name = "Pete"; // works fine
delete user.name; // Error
```

And here we make `user.name` a "forever sealed" constant, just like the built-in `Math.PI`:

```js run
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false,
  configurable: false
});

// won't be able to change user.name or its flags
// all this won't work:
user.name = "Pete";
delete user.name;
Object.defineProperty(user, "name", { value: "Pete" });
```

```smart header="The only attribute change possible: writable true -> false"
There's a minor exception about changing flags.

We can change `writable: true` to `false` for a non-configurable property, thus preventing its value modification (to add another layer of protection). Not the other way around though.
```

## Object.defineProperties

There's a method [Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties) that allows to define many properties at once.

The syntax is:

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

For instance:

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

So, we can set many properties at once.

## Object.getOwnPropertyDescriptors

To get all property descriptors at once, we can use the method [Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors).

Together with `Object.defineProperties` it can be used as a "flags-aware" way of cloning an object:

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

Normally when we clone an object, we use an assignment to copy properties, like this:

```js
for (let key in user) {
  clone[key] = user[key]
}
```

...But that does not copy flags. So if we want a "better" clone then `Object.defineProperties` is preferred.

Another difference is that `for..in` ignores symbolic and non-enumerable properties, but `Object.getOwnPropertyDescriptors` returns *all* property descriptors including symbolic and non-enumerable ones.

## Sealing an object globally

Property descriptors work at the level of individual properties.

There are also methods that limit access to the *whole* object:

[Object.preventExtensions(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
: Forbids the addition of new properties to the object.

[Object.seal(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
: Forbids adding/removing of properties. Sets `configurable: false` for all existing properties.

[Object.freeze(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
: Forbids adding/removing/changing of properties. Sets `configurable: false, writable: false` for all existing properties.

And also there are tests for them:

[Object.isExtensible(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)
: Returns `false` if adding properties is forbidden, otherwise `true`.

[Object.isSealed(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)
: Returns `true` if adding/removing properties is forbidden, and all existing properties have `configurable: false`.

[Object.isFrozen(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)
: Returns `true` if adding/removing/changing properties is forbidden, and all current properties are `configurable: false, writable: false`.

These methods are rarely used in practice.
