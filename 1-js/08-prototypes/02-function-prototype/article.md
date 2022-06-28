# F.prototype

Հեշեք, նոր օբյեկտներ կարող են ստեղծվել կոնստրուկտոր ֆունկցիայով, այսպես՝ `new F()`։

Եթե `F.prototype`-ը օբյեկտ է, ապա `new` օպերատորն այն օգտագործում է նոր օբյեկտի համար `[[Prototype]]` տեղադրելու համար։

```smart
JavaScript-ն ի սկզբանե ուներ նախատիպային ժառանգություն: Դա լեզվի առանցքային առանձնահատկություններից մեկն էր:

Բայց հին ժամանակներում դրան անմիջական մուտք չկար։ Միակ բանը, որն աշխատում էր հուսալիորեն, այս գլխում նկարագրված կոնստրուկտոր ֆունկցիայի `«prototype»` հատկությունն էր: Այնպես որ, շատ սքրիփթներ կան, որտեղ դեռ օգտագործվում է այն:
```

Նկատի ունեցեք, որ `F.prototype`-ը այստեղ նշանակում է `«prototype»` անվանումով սովորական հատկություն `F`-ի համար. Այն հնչում է որպես «prototype» տերմինի նման մի բան, բայց այստեղ մենք իսկապես նկատի ունենք այս անվանումով սովորական հատկություն:

Ահա օրինակը.

```js run
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}

*!*
Rabbit.prototype = animal;
*/!*

let rabbit = new Rabbit("Սպիտակ Նապաստակ"); //  rabbit.__proto__ == animal

alert( rabbit.eats ); // true
```

`Rabbit.prototype = animal` կարգավորումը բառացիորեն նշում է հետևյալը. «երբ `new Rabbit` ստեղծվի, նրա `[[Prototype]]`-ը նշանակել `animal`-ին»։

Ստացված պատկերն այսպիսին է.

![](proto-constructor-animal-rabbit.svg)

Նկարում `«prototype»`-ը հորիզոնական սլաք է, որը նշանակում է կանոնավոր հատկություն, իսկ `[[Prototype]]`-ը ուղղահայաց է՝ նշանակում է `rabbit`-ի ժառանգությունը `animal`-ից:

```smart header="`F.prototype`-ը միայն օգտագործվում է `new F`-ի ժամանակ"
`F.prototype` հատկությունն օգտագործվում է միայն այն դեպքում, երբ `new F` է կանչվում, այն նշանակում է `[[Prototype]]` նոր օբյեկտին։

Եթե ստեղծելուց հետո `F.prototype` հատկությունը փոփոխվի (`F.prototype = <այլ օբյեկտ>`), ապա `new F`-ի կողմից ստեղծված նոր օբյեկտները կունենան մեկ այլ օբյեկտ որպես `[[Prototype]]`, բայց արդեն գոյություն ունեցող օբյեկտները կպահպանեն հինը։
```

## Կանխադրված F.prototype, constructor հատկություն

Յուրաքանչյուր ֆունկցիա ունի `«prototype»` հատկություն, նույնիսկ եթե մենք այն չտրամադրենք:

Կանխադրված `«prototype»`-ն օբյեկտ է, որն ունի միայն մեկ հատկություն՝ `constructor`, որն էլ իր հերթին հղում է կատարում բուն ֆունկցիային։

Այսպես.

```js
function Rabbit() {}

/* կանխադրված prototype-ը
Rabbit.prototype = { constructor: Rabbit };
*/
```

![](function-prototype-constructor.svg)

Մենք կարող ենք ստուգել այն.

```js run
function Rabbit() {}
// կանխադրված․
// Rabbit.prototype = { constructor: Rabbit }

alert( Rabbit.prototype.constructor == Rabbit ); // true
```

Բնականաբար, եթե մենք ոչինչ չանենք, `constructor` հատկությունը հասանելի կլինի բոլոր ճագարներին `[[Prototype]]`-ի միջոցով.

```js run
function Rabbit() {}
// կանխադրված․
// Rabbit.prototype = { constructor: Rabbit }

let rabbit = new Rabbit(); // ժառանգում է {constructor: Rabbit}-ից

alert(rabbit.constructor == Rabbit); // true (prototype-ից)
```

![](rabbit-prototype-constructor.svg)

Մենք կարող ենք օգտագործել արդեն իսկ գոյություն ունեցող օբյեկտի `constructor` հատկությունը՝ նորը ստեղծելու համար։

Ինչպես այստեղ․

```js run
function Rabbit(name) {
  this.name = name;
  alert(name);
}

let rabbit = new Rabbit("Սպիտակ Ճագար");

*!*
let rabbit2 = new rabbit.constructor("Սև Ճագար");
*/!*
```

That's handy when we have an object, don't know which constructor was used for it (e.g. it comes from a 3rd party library), and we need to create another one of the same kind.

But probably the most important thing about `"constructor"` is that...

**...JavaScript itself does not ensure the right `"constructor"` value.**

Yes, it exists in the default `"prototype"` for functions, but that's all. What happens with it later -- is totally on us.

In particular, if we replace the default prototype as a whole, then there will be no `"constructor"` in it.

For instance:

```js run
function Rabbit() {}
Rabbit.prototype = {
  jumps: true
};

let rabbit = new Rabbit();
*!*
alert(rabbit.constructor === Rabbit); // false
*/!*
```

So, to keep the right `"constructor"` we can choose to add/remove properties to the default `"prototype"` instead of overwriting it as a whole:

```js
function Rabbit() {}

// Not overwrite Rabbit.prototype totally
// just add to it
Rabbit.prototype.jumps = true
// the default Rabbit.prototype.constructor is preserved
```

Or, alternatively, recreate the `constructor` property manually:

```js
Rabbit.prototype = {
  jumps: true,
*!*
  constructor: Rabbit
*/!*
};

// now constructor is also correct, because we added it
```


## Summary

In this chapter we briefly described the way of setting a `[[Prototype]]` for objects created via a constructor function. Later we'll see more advanced programming patterns that rely on it.

Everything is quite simple, just a few notes to make things clear:

- The `F.prototype` property (don't mistake it for `[[Prototype]]`) sets `[[Prototype]]` of new objects when `new F()` is called.
- The value of `F.prototype` should be either an object or `null`: other values won't work.
-  The `"prototype"` property only has such a special effect when set on a constructor function, and invoked with `new`.

On regular objects the `prototype` is nothing special:
```js
let user = {
  name: "John",
  prototype: "Bla-bla" // no magic at all
};
```

By default all functions have `F.prototype = { constructor: F }`, so we can get the constructor of an object by accessing its `"constructor"` property.
