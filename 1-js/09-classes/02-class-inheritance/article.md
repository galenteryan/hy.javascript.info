
# Class-ի ժառանգում

Class-ի ժառանգումը միջոց է, երբ մի class-ն ընդլայնվում է մեկ այլ class-ից:

Այդպես, գոյություն ունեցող class-ի հիման վրա մենք կարող ենք ստեղծել նոր ֆունկցիոնալ:

## «Extends» հիմնաբառը

Ենթադրենք, մենք ունենք `Animal` class.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  run(speed) {
    this.speed = speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }
  stop() {
    this.speed = 0;
    alert(`${this.name} stands still.`);
  }
}

let animal = new Animal("My animal");
```

Ահա թե ինչպես կարող ենք գրաֆիկորեն ներկայացնել `animal` օբյեկտը և `Animal` class-ը.

![](rabbit-animal-independent-animal.svg)

...Եվ մեզ անհրաժեշտ է ստեղծել ևս մեկը՝ `class Rabbit`:

Քանի որ ճագարները կենդանիներ են, ապա `Rabbit` class-ը պետք է հիմնված լինի `Animal`-ի վրա, հասանելիություն ունենա կենդանիների մեթոդներին, որպեսզի ճագարները կարողանան անել այն, ինչ կարող են անել «ընդհանուր» կենդանիները:

Մեկ այլ class-ից ընդլայնվելու շարահյուսությունը հետևյալն է. `class Child extends Parent`:

Եկեք ստեղծենք `class Rabbit`, որը ժառանգում է `Animal`-ից.

```js
*!*
class Rabbit extends Animal {
*/!*
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.hide(); // White Rabbit hides!
```

`Rabbit` class-ի օբյեկտը հասանելիություն ունի ինչպես `Rabbit`-ի մեթոդներին, օրինակ՝ `rabbit.hide()`, այնպես էլ `Animal`-ի մեթոդներին, օրինակ՝ `rabbit.run()`:

Ներքին առումով, `extends` հիմնաբառը աշխատում է հին ու բարի նախատիպի մեխանիկայով: Այն `Rabbit.prototype.[[Prototype]]`-ը սահմանում է `Animal.prototype`: Այսպիսով, եթե մեթոդը չի գտնվել `Rabbit.prototype`-ում, JavaScript-ը վերցնում է այն `Animal.prototype`-ից:

![](animal-rabbit-extends.svg)

Օրինակ՝ `rabbit.run` մեթոդը գտնելու համար շարժիչը ստուգում է (նկարում՝ ներքևից վերև).
1. `rabbit` օբյեկտը (չունի `run`)։
2. Դրա նախատիպը, որը `Rabbit.prototype`-ն է (ունի `hide`, բայց չունի `run`)։
3. Դրա նախատիպը, որը (շնորհիվ `extends`-ի) `Animal.prototype`-ն է, վերջապես ունի `run` մեթոդ:

Ինչպես հիշում ենք <info:native-prototypes> գլխից, JavaScript-ն ինքնին օգտագործում է նախատիպային ժառանգություն ներկառուցված օբյեկտների համար: Օրինակ՝ `Date.prototype.[[Prototype]]`-ը `Object.prototype`-ն է: Ահա թե ինչու ամսաթվերն ունեն հասանելիություն ընդհանուր օբյեկտների մեթոդներին:

````smart header="Ցանկացած արտահայտություն թույլատրվում է `extends`-ից հետո"
Class-ի շարահյուսությունը թույլ է տալիս նշել ոչ միայն class-ը, այլ ցանկացած արտահայտություն `extends`-ից հետո:

Օրինակ՝ ֆունկցիայի կանչը, որը ստեղծում է ծնող class-ը.

```js run
function f(phrase) {
  return class {
    sayHi() { alert(phrase); }
  };
}

*!*
class User extends f("Hello") {}
*/!*

new User().sayHi(); // Hello
```
Այստեղ `class User`-ը ժառանգում է `f("Hello")`-ի վերադարձրած արդյունքից:

Դա կարող է օգտակար լինել ծրագրավորման առաջադեմ օրինաչափությունների համար, երբ մենք օգտագործում ենք ֆունկցիաներ՝ բազմաթիվ պայմաններից կախված class-ներ ստեղծելու և դրանցից ժառանգելու համար:
````

## Գերակայող մեթոդ

Հիմա եկեք առաջ շարժվենք և անտեսենք մի մեթոդ: Կանխադրված կերպով այն մեթոդները, որոնք նշված չեն `class Rabbit`-ում, ուղղակիորեն վերցվում են `class Animal`-ից՝ «Ինչպես որ կա»:

Բայց, եթե մենք `Rabbit`-ում նշենք մեր սեփական մեթոդը, ինչպիսին է `stop()`-ը, ապա փոխարենը դա կօգտագործվի.

```js
class Rabbit extends Animal {
  stop() {
    // ...այժմ սա կօգտագործվի rabbit.stop()-ի համար
    // Animal class-ի stop()-ի փոխարեն
  }
}
```

Այնուամենայնիվ, սովորաբար մենք չենք ցանկանում ամբողջությամբ փոխարինել ծնողի մեթոդը, այլ ավելի շուտ ցանկանում ենք կառուցել դրա վրա՝ փոփոխելով կամ ընդլայնելով ֆունկցիոնալը: Մենք ինչ-որ բան ենք անում մեր մեթոդով, բայց դրանից առաջ/հետո կամ ընթացքում կանչում ենք ծնողի մեթոդը:

Class-ները դրա համար տրամադրում են `«super»` հիմնաբառը:

- `super.method(...)` ծնողի մեթոդը կանչելու համար
- `super(...)` ծնողի կոնստրուկտորը կանչելու համար (միայն մեր կոնստրուկտորի ներսում).

Օրինակ՝ թող, որ մեր ճագարը թաքնվի, երբ կանգնում է.

```js run
class Animal {

  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  run(speed) {
    this.speed = speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }

  stop() {
    this.speed = 0;
    alert(`${this.name} stands still.`);
  }

}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

*!*
  stop() {
    super.stop(); // call parent stop
    this.hide(); // and then hide
  }
*/!*
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.stop(); // White Rabbit stands still. White Rabbit hides!
```

Այժմ `Rabbit`-ն ունի `stop` մեթոդը, որն ընթացքում կանչում է ծնողինը՝ `super.stop()`:

````smart header="Սլաքով ֆունկցիաները չունեն `super`"
Ինչպես նշվեց <info:arrow-functions> գլխում, սլաքով ֆունկցիաները չունեն `super`:

Հասանելիության դեպքում այն վերցվում է արտաքին ֆունկցիայից: Օրինակ․

```js
class Rabbit extends Animal {
  stop() {
    setTimeout(() => super.stop(), 1000); // կանչում ենք ծնողի stop-ը 1 վրկ անց
  }
}
```

Սլաքով ֆունկցիայում `super`-ը նույնն է, ինչ `stop()`-ում, ուստի այն աշխատում է այնպես, ինչպես նախատեսված է: Եթե այստեղ նշեինք «սովորական» ֆունկցիա, ապա սխալ կլիներ.

```js
// Անսպասելի super
setTimeout(function() { super.stop() }, 1000);
```
````

## Գերակայող կոնստրուկտոր

Կոնստրուկտորների դեպքում դա մի փոքր բարդանում է:

Մինչ այս `Rabbit`-ը սեփական `constructor` չուներ։

Համաձայն [հատկանշման](https://tc39.github.io/ecma262/#sec-runtime-semantics-classdefinitionevaluation), եթե class-ը ընդլայնվում է մեկ այլ class-ից և չունի `constructor`, ապա հետևյալ «դատարկ» `constructor`-ն է ստեղծվում.

```js
class Rabbit extends Animal {
  // ստեղծվում է առանց սեփական կոնստրուկտորով class-ների ընդլայնման համար
*!*
  constructor(...args) {
    super(...args);
  }
*/!*
}
```

Ինչպես տեսնում ենք, այն հիմնականում կանչում է ծնող `constructor`-ը՝ փոխանցելով նրան բոլոր արգումենտները: Դա տեղի է ունենում, եթե մենք չգրենք մեր սեփական կոնստրուկտորը:

Հիմա եկեք `Rabbit`-ին ավելացնենք սեփական կոնստրուկտոր: Այն կսահմանի նաև `earLength`-ը՝ ի լրում `name`-ին:

```js run
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  // ...
}

class Rabbit extends Animal {

*!*
  constructor(name, earLength) {
    this.speed = 0;
    this.name = name;
    this.earLength = earLength;
  }
*/!*

  // ...
}

*!*
// Չի աշխատում
let rabbit = new Rabbit("White Rabbit", 10); // Error: this is not defined.
*/!*
```

Վա՜յ։ Մենք սխալ ունենք: Հիմա մենք չենք կարող ճագարներ ստեղծել: Ի՞նչը սխալ գնաց։

Կարճ պատասխանն է.

- **Ժառանգող class-ների կոնստրուկտորները պետք է կանչեն `super(...)`-ը, և (!) դա պետք է անեն նախքան `this`-ի օգտագործումը:**

...Բայց ինչո՞ւ։ Ի՞նչ է այստեղ կատարվում։ Իրոք, պահանջը տարօրինակ է թվում։

Իհարկե, բացատրություն կա։ Եկեք մանրամասնենք այնպես, որ իսկապես հասկանալի լինի, թե ինչ է կատարվում:

JavaScript-ում ժառանգող class-ի կոնստրուկտոր ֆունկցիայի (այսպես կոչված «ածանցյալ կոնստրուկտոր») և այլ ֆունկցիաների միջև կա տարբերություն: Ստացված կոնստրուկտորն ունի հատուկ ներքին հատկություն՝ `[[ConstructorKind]]:"derived"`: Դա հատուկ ներքին պիտակ է:

Այդ պիտակն ազդում է նրա վարքագծի վրա՝ `new`-ի միջոցով:

- Երբ սովորական ֆունկցիան գործարկվում է `new`-ով, այն ստեղծում է դատարկ օբյեկտ և վերագրում այն `this`-ին:
- Բայց երբ ածանցյալ կոնստրուկտորն է աշխատում, այն չի անում դա: Այն ակնկալում է, որ ծնող կոնստրուկտորը կանի այդ աշխատանքը:

Այսպիսով, ածանցյալ կոնստրուկտորը պետք է կանչի `super`՝ իր ծնող (բազային) կոնստրուկտորը գործարկելու համար, հակառակ դեպքում `this`-ի համար օբյեկտ չի ստեղծվի և մենք սխալ կստանանք:

Որպեսզի `Rabbit` կոնստրուկտորն աշխատի, այն պետք է կանչի `super()`՝ `this`-ն օգտագործելուց առաջ, ինչպես այստեղ.

```js run
class Animal {

  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  // ...
}

class Rabbit extends Animal {

  constructor(name, earLength) {
*!*
    super(name);
*/!*
    this.earLength = earLength;
  }

  // ...
}

*!*
// այժմ հիանալի է
let rabbit = new Rabbit("White Rabbit", 10);
alert(rabbit.name); // White Rabbit
alert(rabbit.earLength); // 10
*/!*
```

### Overriding class fields: a tricky note

```warn header="Advanced note"
This note assumes you have a certain experience with classes, maybe in other programming languages.

It provides better insight into the language and also explains the behavior that might be a source of bugs (but not very often).

If you find it difficult to understand, just go on, continue reading, then return to it some time later.
```

We can override not only methods, but also class fields.

Although, there's a tricky behavior when we access an overridden field in parent constructor, quite different from most other programming languages.

Consider this example:

```js run
class Animal {
  name = 'animal';

  constructor() {
    alert(this.name); // (*)
  }
}

class Rabbit extends Animal {
  name = 'rabbit';
}

new Animal(); // animal
*!*
new Rabbit(); // animal
*/!*
```

Here, class `Rabbit` extends `Animal` and overrides the `name` field with its own value.

There's no own constructor in `Rabbit`, so `Animal` constructor is called.

What's interesting is that in both cases: `new Animal()` and `new Rabbit()`, the `alert` in the line `(*)` shows `animal`.

**In other words, the parent constructor always uses its own field value, not the overridden one.**

What's odd about it?

If it's not clear yet, please compare with methods.

Here's the same code, but instead of `this.name` field we call `this.showName()` method:

```js run
class Animal {
  showName() {  // instead of this.name = 'animal'
    alert('animal');
  }

  constructor() {
    this.showName(); // instead of alert(this.name);
  }
}

class Rabbit extends Animal {
  showName() {
    alert('rabbit');
  }
}

new Animal(); // animal
*!*
new Rabbit(); // rabbit
*/!*
```

Please note: now the output is different.

And that's what we naturally expect. When the parent constructor is called in the derived class, it uses the overridden method.

...But for class fields it's not so. As said, the parent constructor always uses the parent field.

Why is there a difference?

Well, the reason is the field initialization order. The class field is initialized:
- Before constructor for the base class (that doesn't extend anything),
- Immediately after `super()` for the derived class.

In our case, `Rabbit` is the derived class. There's no `constructor()` in it. As said previously, that's the same as if there was an empty constructor with only `super(...args)`.

So, `new Rabbit()` calls `super()`, thus executing the parent constructor, and (per the rule for derived classes) only after that its class fields are initialized. At the time of the parent constructor execution, there are no `Rabbit` class fields yet, that's why `Animal` fields are used.

This subtle difference between fields and methods is specific to JavaScript.

Luckily, this behavior only reveals itself if an overridden field is used in the parent constructor. Then it may be difficult to understand what's going on, so we're explaining it here.

If it becomes a problem, one can fix it by using methods or getters/setters instead of fields.

## Super: internals, [[HomeObject]]

```warn header="Advanced information"
If you're reading the tutorial for the first time - this section may be skipped.

It's about the internal mechanisms behind inheritance and `super`.
```

Let's get a little deeper under the hood of `super`. We'll see some interesting things along the way.

First to say, from all that we've learned till now, it's impossible for `super` to work at all!

Yeah, indeed, let's ask ourselves, how it should technically work? When an object method runs, it gets the current object as `this`. If we call `super.method()` then, the engine needs to get the `method` from the prototype of the current object. But how?

The task may seem simple, but it isn't. The engine knows the current object `this`, so it could get the parent `method` as `this.__proto__.method`. Unfortunately, such a "naive" solution won't work.

Let's demonstrate the problem. Without classes, using plain objects for the sake of simplicity.

You may skip this part and go below to the `[[HomeObject]]` subsection if you don't want to know the details. That won't harm. Or read on if you're interested in understanding things in-depth.

In the example below, `rabbit.__proto__ = animal`. Now let's try: in `rabbit.eat()` we'll call `animal.eat()`, using `this.__proto__`:

```js run
let animal = {
  name: "Animal",
  eat() {
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {
*!*
    // that's how super.eat() could presumably work
    this.__proto__.eat.call(this); // (*)
*/!*
  }
};

rabbit.eat(); // Rabbit eats.
```

At the line `(*)` we take `eat` from the prototype (`animal`) and call it in the context of the current object. Please note that `.call(this)` is important here, because a simple `this.__proto__.eat()` would execute parent `eat` in the context of the prototype, not the current object.

And in the code above it actually works as intended: we have the correct `alert`.

Now let's add one more object to the chain. We'll see how things break:

```js run
let animal = {
  name: "Animal",
  eat() {
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  eat() {
    // ...bounce around rabbit-style and call parent (animal) method
    this.__proto__.eat.call(this); // (*)
  }
};

let longEar = {
  __proto__: rabbit,
  eat() {
    // ...do something with long ears and call parent (rabbit) method
    this.__proto__.eat.call(this); // (**)
  }
};

*!*
longEar.eat(); // Error: Maximum call stack size exceeded
*/!*
```

The code doesn't work anymore! We can see the error trying to call `longEar.eat()`.

It may be not that obvious, but if we trace `longEar.eat()` call, then we can see why. In both lines `(*)` and `(**)` the value of `this` is the current object (`longEar`). That's essential: all object methods get the current object as `this`, not a prototype or something.

So, in both lines `(*)` and `(**)` the value of `this.__proto__` is exactly the same: `rabbit`. They both call `rabbit.eat` without going up the chain in the endless loop.

Here's the picture of what happens:

![](this-super-loop.svg)

1. Inside `longEar.eat()`, the line `(**)` calls `rabbit.eat` providing it with `this=longEar`.
    ```js
    // inside longEar.eat() we have this = longEar
    this.__proto__.eat.call(this) // (**)
    // becomes
    longEar.__proto__.eat.call(this)
    // that is
    rabbit.eat.call(this);
    ```
2. Then in the line `(*)` of `rabbit.eat`, we'd like to pass the call even higher in the chain, but `this=longEar`, so `this.__proto__.eat` is again `rabbit.eat`!

    ```js
    // inside rabbit.eat() we also have this = longEar
    this.__proto__.eat.call(this) // (*)
    // becomes
    longEar.__proto__.eat.call(this)
    // or (again)
    rabbit.eat.call(this);
    ```

3. ...So `rabbit.eat` calls itself in the endless loop, because it can't ascend any further.

The problem can't be solved by using `this` alone.

### `[[HomeObject]]`

To provide the solution, JavaScript adds one more special internal property for functions: `[[HomeObject]]`.

When a function is specified as a class or object method, its `[[HomeObject]]` property becomes that object.

Then `super` uses it to resolve the parent prototype and its methods.

Let's see how it works, first with plain objects:

```js run
let animal = {
  name: "Animal",
  eat() {         // animal.eat.[[HomeObject]] == animal
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {         // rabbit.eat.[[HomeObject]] == rabbit
    super.eat();
  }
};

let longEar = {
  __proto__: rabbit,
  name: "Long Ear",
  eat() {         // longEar.eat.[[HomeObject]] == longEar
    super.eat();
  }
};

*!*
// works correctly
longEar.eat();  // Long Ear eats.
*/!*
```

It works as intended, due to `[[HomeObject]]` mechanics. A method, such as `longEar.eat`, knows its `[[HomeObject]]` and takes the parent method from its prototype. Without any use of `this`.

### Methods are not "free"

As we've known before, generally functions are "free", not bound to objects in JavaScript. So they can be copied between objects and called with another `this`.

The very existence of `[[HomeObject]]` violates that principle, because methods remember their objects. `[[HomeObject]]` can't be changed, so this bond is forever.

The only place in the language where `[[HomeObject]]` is used -- is `super`. So, if a method does not use `super`, then we can still consider it free and copy between objects. But with `super` things may go wrong.

Here's the demo of a wrong `super` result after copying:

```js run
let animal = {
  sayHi() {
    alert(`I'm an animal`);
  }
};

// rabbit inherits from animal
let rabbit = {
  __proto__: animal,
  sayHi() {
    super.sayHi();
  }
};

let plant = {
  sayHi() {
    alert("I'm a plant");
  }
};

// tree inherits from plant
let tree = {
  __proto__: plant,
*!*
  sayHi: rabbit.sayHi // (*)
*/!*
};

*!*
tree.sayHi();  // I'm an animal (?!?)
*/!*
```

A call to `tree.sayHi()` shows "I'm an animal". Definitely wrong.

The reason is simple:
- In the line `(*)`, the method `tree.sayHi` was copied from `rabbit`. Maybe we just wanted to avoid code duplication?
- Its `[[HomeObject]]` is `rabbit`, as it was created in `rabbit`. There's no way to change `[[HomeObject]]`.
- The code of `tree.sayHi()` has `super.sayHi()` inside. It goes up from `rabbit` and takes the method from `animal`.

Here's the diagram of what happens:

![](super-homeobject-wrong.svg)

### Methods, not function properties

`[[HomeObject]]` is defined for methods both in classes and in plain objects. But for objects, methods must be specified exactly as `method()`, not as `"method: function()"`.

The difference may be non-essential for us, but it's important for JavaScript.

In the example below a non-method syntax is used for comparison. `[[HomeObject]]` property is not set and the inheritance doesn't work:

```js run
let animal = {
  eat: function() { // intentionally writing like this instead of eat() {...
    // ...
  }
};

let rabbit = {
  __proto__: animal,
  eat: function() {
    super.eat();
  }
};

*!*
rabbit.eat();  // Error calling super (because there's no [[HomeObject]])
*/!*
```

## Summary

1. To extend a class: `class Child extends Parent`:
    - That means `Child.prototype.__proto__` will be `Parent.prototype`, so methods are inherited.
2. When overriding a constructor:
    - We must call parent constructor as `super()` in `Child` constructor before using `this`.
3. When overriding another method:
    - We can use `super.method()` in a `Child` method to call `Parent` method.
4. Internals:
    - Methods remember their class/object in the internal `[[HomeObject]]` property. That's how `super` resolves parent methods.
    - So it's not safe to copy a method with `super` from one object to another.

Also:
- Arrow functions don't have their own `this` or `super`, so they transparently fit into the surrounding context.
