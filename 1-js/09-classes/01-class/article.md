
# Class․ պարզ շարահյուսություն

```quote author="Wikipedia"
Օբյեկտ-կողմնորոշված ծրագրավորման մեջ *class*-ը ընդարձակվող ծրագրային կոդի նմուշ է՝ օբյեկտներ ստեղծելու համար, որը տրամադրում է նախնական արժեքներ կարգավիճակի (անդամ փոփոխականներ) և պահելաձևի իրականացման համար (անդամ ֆունկցիաներ կամ մեթոդներ):
```

Գործնականում մենք հաճախ նույն տեսակի բազմաթիվ օբյեկտներ ստեղծելու կարիք ենք ունենում, օրինակ՝ օգտատերեր, ապրանքներ կամ որևէ այլ բան:

Ինչպես արդեն գիտենք <info:constructor-new> գլխից, `new function`-ը կարող է օգնել այդ հարցում:

Սակայն ժամանակակից JavaScript-ում կա ավելի առաջադեմ «class» կոնստրուկցիա, որը ներկայացնում է հիանալի նոր հնարավորություններ, որոնք օգտակար են օբյեկտ-կողմնորոշված ծրագրավորման համար:

## «Class»-ի շարահյուսությունը

Պարզ շարահյուսությունը հետևյալն է․
```js
class MyClass {
  // class-ի մեթոդներ
  constructor() { ... }
  method1() { ... }
  method2() { ... }
  method3() { ... }
  ...
}
```

Այնուհետև օգտագործեք `new MyClass()`՝ թվարկված բոլոր մեթոդներով նոր օբյեկտ ստեղծելու համար:

`constructor()` մեթոդը ավտոմատ կերպով կանչվում է `new`-ի կողմից, այնպես որ մենք կարող ենք այնտեղ սկզբնավորել օբյեկտը:

Օրինակ․

```js run
class User {

  constructor(name) {
    this.name = name;
  }

  sayHi() {
    alert(this.name);
  }

}

// Օգտագործում
let user = new User("John");
user.sayHi();
```

Երբ `new User("John")`-ը կոչվում է.
1. Ստեղծվում է նոր օբյեկտ:
2. `constructor`-ն աշխատում է տրված արգումենտով և վերագրում այն `this.name`-ին:

...Այնուհետև մենք կարող ենք կանչել օբյեկտի մեթոդներ, ինչպիսին `user.sayHi()`-ն է:


```warn header="Ստորակետ չկա class-ի մեթոդների միջև"
Սկսնակ ծրագրավորողների մոտ տարածված սխալը class-ի մեթոդների միջև ստորակետ դնելն է, ինչը կհանգեցնի շարահյուսական սխալի:

Այստեղի նշումը չպետք է շփոթել օբյեկտի գրելաձևի հետ: Ստորակետեր չեն պահանջվում class-ում:
```

## Ի՞նչ է class-ը

Այսպիսով, ի՞նչ է իրականում `class`-ը: Դա բոլորովին նոր լեզվական մակարդակ չէ, ինչպես կարող է թվալ:

Եկեք բացահայտենք ցանկացած մոգություն և տեսնենք, թե իրականում ինչ է class-ը: Դա կօգնի հասկանալ բազմաթիվ բարդ ասպեկտներ:

JavaScript-ում class-ը ֆունկցիայի տեսակ է:

Ահա, տեսեք.

```js run
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// ապացույց՝ User-ը ֆունկցիա է
*!*
alert(typeof User); // function
*/!*
```

Այն, ինչ իրականում անում է `class User {...}` կոնստրուկցիան, հետևյալն է.

1. Ստեղծում է `User` անունով ֆունկցիա, որը դառնում է class-ի հայտարարագրման արդյունք: Ֆունկցիայի կոդը վերցված է `constructor` մեթոդից (եթե նման մեթոդ չգրենք, համարվում է դատարկ):
2. Class-ի մեթոդները, ինչպիսին է `sayHi`-ը, պահպանվում են `User.prototype`-ում:

`new User` օբյեկտի ստեղծումից հետո, երբ մենք կանչում ենք դրա մեթոդը, այն վերցվում է նախատիպից, ինչպես նկարագրված է <info:function-prototype> գլխում: Այսպիսով, օբյեկտն ունի հասանելիություն class-ի մեթոդներին:

`class User` հայտարարագրի արդյունքը կարող ենք ներկայացնել հետևյալ կերպ.

![](class-user.svg)

Ահա կոդ՝ այդ ամենը ներհայեցելու և վերլուծելու համար.

```js run
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// class-ը ֆունկցիա է
alert(typeof User); // function

// ...կամ, ավելի ճիշտ, կոնստրուկտոր մեթոդ է
alert(User === User.prototype.constructor); // true

// Մեթոդները User.prototype-ում են, օր․՝
alert(User.prototype.sayHi); // sayHi մեթոդի կոդը

// նախատիպի մեջ կա հենց երկու մեթոդ
alert(Object.getOwnPropertyNames(User.prototype)); // constructor, sayHi
```

## Ոչ միայն շարահյուսական շաքար

Երբեմն մարդիկ ասում են, որ `class`-ը «շարահյուսական շաքար» է (շարահյուսություն, որը նախատեսված է իրերն ավելի հեշտ ընթեռնելու համար, բայց ոչ մի նոր բան չի ներկայացնում իրենից), քանի որ մենք իրականում կարող ենք նույն բանը հայտարարել առանց `class` հիմնաբառի օգտագործման:

```js run
// User class-ը վերաշարադրենք մաքուր ֆունկցիաներով

// 1. Ստեղծենք կոնստրուկտոր ֆունկցիա
function User(name) {
  this.name = name;
}
// ֆունկցիայի նախատիպը լռելյայն ունի «կոնստրուկտոր» հատկություն,
// այնպես որ, մենք այն ստեղծելու կարիք չունենք

// 2. Ավելացնենք մեթոդը նախատիպում
User.prototype.sayHi = function() {
  alert(this.name);
};

// Օգտագործում․
let user = new User("John");
user.sayHi();
```

The result of this definition is about the same. So, there are indeed reasons why `class` can be considered a syntactic sugar to define a constructor together with its prototype methods.

Still, there are important differences.

1. First, a function created by `class` is labelled by a special internal property `[[IsClassConstructor]]: true`. So it's not entirely the same as creating it manually.

    The language checks for that property in a variety of places. For example, unlike a regular function, it must be called with `new`:

    ```js run
    class User {
      constructor() {}
    }

    alert(typeof User); // function
    User(); // Error: Class constructor User cannot be invoked without 'new'
    ```

    Also, a string representation of a class constructor in most JavaScript engines starts with the "class..."

    ```js run
    class User {
      constructor() {}
    }

    alert(User); // class User { ... }
    ```
    There are other differences, we'll see them soon.

2. Class methods are non-enumerable.
    A class definition sets `enumerable` flag to `false` for all methods in the `"prototype"`.

    That's good, because if we `for..in` over an object, we usually don't want its class methods.

3. Classes always `use strict`.
    All code inside the class construct is automatically in strict mode.

Besides, `class` syntax brings many other features that we'll explore later.

## Class Expression

Just like functions, classes can be defined inside another expression, passed around, returned, assigned, etc.

Here's an example of a class expression:

```js
let User = class {
  sayHi() {
    alert("Hello");
  }
};
```

Similar to Named Function Expressions, class expressions may have a name.

If a class expression has a name, it's visible inside the class only:

```js run
// "Named Class Expression"
// (no such term in the spec, but that's similar to Named Function Expression)
let User = class *!*MyClass*/!* {
  sayHi() {
    alert(MyClass); // MyClass name is visible only inside the class
  }
};

new User().sayHi(); // works, shows MyClass definition

alert(MyClass); // error, MyClass name isn't visible outside of the class
```

We can even make classes dynamically "on-demand", like this:

```js run
function makeClass(phrase) {
  // declare a class and return it
  return class {
    sayHi() {
      alert(phrase);
    }
  };
}

// Create a new class
let User = makeClass("Hello");

new User().sayHi(); // Hello
```


## Getters/setters

Just like literal objects, classes may include getters/setters, computed properties etc.

Here's an example for `user.name` implemented using `get/set`:

```js run
class User {

  constructor(name) {
    // invokes the setter
    this.name = name;
  }

*!*
  get name() {
*/!*
    return this._name;
  }

*!*
  set name(value) {
*/!*
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }

}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Name is too short.
```

Technically, such class declaration works by creating getters and setters in `User.prototype`.

## Computed names [...]

Here's an example with a computed method name using brackets `[...]`:

```js run
class User {

*!*
  ['say' + 'Hi']() {
*/!*
    alert("Hello");
  }

}

new User().sayHi();
```

Such features are easy to remember, as they resemble that of literal objects.

## Class fields

```warn header="Old browsers may need a polyfill"
Class fields are a recent addition to the language.
```

Previously, our classes only had methods.

"Class fields" is a syntax that allows to add any properties.

For instance, let's add `name` property to `class User`:

```js run
class User {
*!*
  name = "John";
*/!*

  sayHi() {
    alert(`Hello, ${this.name}!`);
  }
}

new User().sayHi(); // Hello, John!
```

So, we just write "<property name> = <value>" in the declaration, and that's it.

The important difference of class fields is that they are set on individual objects, not `User.prototype`:

```js run
class User {
*!*
  name = "John";
*/!*
}

let user = new User();
alert(user.name); // John
alert(User.prototype.name); // undefined
```

We can also assign values using more complex expressions and function calls:

```js run
class User {
*!*
  name = prompt("Name, please?", "John");
*/!*
}

let user = new User();
alert(user.name); // John
```


### Making bound methods with class fields

As demonstrated in the chapter <info:bind> functions in JavaScript have a dynamic `this`. It depends on the context of the call.

So if an object method is passed around and called in another context, `this` won't be a reference to its object any more.

For instance, this code will show `undefined`:

```js run
class Button {
  constructor(value) {
    this.value = value;
  }

  click() {
    alert(this.value);
  }
}

let button = new Button("hello");

*!*
setTimeout(button.click, 1000); // undefined
*/!*
```

The problem is called "losing `this`".

There are two approaches to fixing it, as discussed in the chapter <info:bind>:

1. Pass a wrapper-function, such as `setTimeout(() => button.click(), 1000)`.
2. Bind the method to object, e.g. in the constructor.

Class fields provide another, quite elegant syntax:

```js run
class Button {
  constructor(value) {
    this.value = value;
  }
*!*
  click = () => {
    alert(this.value);
  }
*/!*
}

let button = new Button("hello");

setTimeout(button.click, 1000); // hello
```

The class field `click = () => {...}` is created on a per-object basis, there's a separate function for each `Button` object, with `this` inside it referencing that object. We can pass `button.click` around anywhere, and the value of `this` will always be correct.

That's especially useful in browser environment, for event listeners.

## Summary

The basic class syntax looks like this:

```js
class MyClass {
  prop = value; // property

  constructor(...) { // constructor
    // ...
  }

  method(...) {} // method

  get something(...) {} // getter method
  set something(...) {} // setter method

  [Symbol.iterator]() {} // method with computed name (symbol here)
  // ...
}
```

`MyClass` is technically a function (the one that we provide as `constructor`), while methods, getters and setters are written to `MyClass.prototype`.

In the next chapters we'll learn more about classes, including inheritance and other features.
