
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

Երբեմն մարդիկ ասում են, որ `class`-ը «շարահյուսական շաքար» է (շարահյուսություն, որը նախատեսված է իրերն ավելի հեշտ ընթեռնելու համար, բայց իրենից ոչ մի նոր բան չի ներկայացնում), քանի որ մենք իրականում կարող ենք նույն բանը հայտարարել առանց `class` հիմնաբառի օգտագործման:

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

Այս սահմանման արդյունքը մոտավորապես նույնն է: Այսպիսով, իսկապես կան պատճառներ, թե ինչու կարելի է `class`-ը համարել շարահյուսական շաքար՝ կոնստրուկտորը իր նախատիպային մեթոդների հետ միասին սահմանելու համար:

Այնուամենայնիվ, կան կարևոր տարբերություններ.

1. Նախ, `class`-ի կողմից ստեղծված ֆունկցիան պիտակավորված է հատուկ ներքին հատկությամբ՝ `[[IsClassConstructor]]: true`: Այսպիսով, դա ամբողջովին նույնը չէ, ինչ ձեռքով ստեղծելը:

    Լեզուն ստուգում է այդ հատկությունը տարբեր վայրերում: Օրինակ․ ի տարբերություն սովորական ֆունկցիայի, այն պետք է կանչվի `new`-ով.

    ```js run
    class User {
      constructor() {}
    }

    alert(typeof User); // function
    User(); // Error: Class constructor User cannot be invoked without 'new'
    ```

    Նաև JavaScript շարժիչների մեծ մասում class կոնստրուկտորի տողային ներկայացումը սկսվում է class...»-ով:

    ```js run
    class User {
      constructor() {}
    }

    alert(User); // class User { ... }
    ```
    Կան նաև այլ տարբերություններ, մենք շուտով կդիտարկենք դրանք:

2. Class-ի մեթոդներն անթվարկելի են։
    Class-ի սահմանումը `«prototype»`-ի բոլոր մեթոդների համար `enumerable` դրոշակը դնում է `false`:

    Դա լավ է, քանի որ, եթե մենք `for..in` ցիկլ կազմենք օբյեկտի համար, մեզ սովորաբար անհրաժեշտ չեն լինում դրա class-ի մեթոդները:

3. Class-ները միշտ `use strict` են։
    Class կոնստրուկցիայի ներսում գտնվող ամբողջ կոդը ավտոմատ կերպով գտնվում է խիստ ռեժիմում:

Բացի այդ, `class` շարահյուսությունը հանգեցնում է բազմաթիվ այլ առանձնահատկությունների, որոնք մենք հետագայում կուսումնասիրենք:

## Class Expression

Ինչպես ֆունկցիաները, այնպես էլ class-ները կարող են սահմանվել մեկ այլ արտահայտության ներսում, փոխանցվել, վերադարձվել, նշանակվել և այլն․․․

Ահա class-ի արտահայտության օրինակ.

```js
let User = class {
  sayHi() {
    alert("Hello");
  }
};
```

Անվանված Ֆունկցիոնալ Արտահայտությունների պես, class-ի արտահայտությունները նույնպես կարող են անվանում ունենալ:

Եթե class-ի արտահայտությունն ունի անվանում, այն տեսանելի է միայն class-ի ներսում.

```js run
// "Named Class Expression"
// (հատկորոշման մեջ նման տերմին չկա, բայց դա նման է Named Function Expression-ին)
let User = class *!*MyClass*/!* {
  sayHi() {
    alert(MyClass); // MyClass անվանումը տեսանելի է միայն class-ի ներսում
  }
};

new User().sayHi(); // աշխատում է, ցույց է տալիս MyClass-ի սահմանումը

alert(MyClass); // սխալ, MyClass անվանումը տեսանելի չէ class-ից դուրս
```

We can even make classes dynamically "on-demand", like this:
Մենք նույնիսկ կարող ենք class-ները դինամիկ դարձնել՝ «ըստ պահանջի», այսպես.

```js run
function makeClass(phrase) {
  // հայտարարենք class և վերադարձնենք այն
  return class {
    sayHi() {
      alert(phrase);
    }
  };
}

// Ստեղծենք նոր class
let User = makeClass("Hello");

new User().sayHi(); // Hello
```


## Գեթթերներ/սեթթերներ

Ինչպես բառացի օբյեկտները, այնպես էլ class-ները կարող են ներառել գեթթերներ/սեթթերներ, հաշվարկված հատկություններ և այլն․․․

Ահա `user.name`-ի օրինակ, որն իրականացվել է `get/set`-ի միջոցով.

```js run
class User {

  constructor(name) {
    // կանչում է սեթթերը
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
      alert("Անունը չափազանց կարճ է:");
      return;
    }
    this._name = value;
  }

}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Անունը չափազանց կարճ է:
```

Տեխնիկապես, այդպիսի հայտարարագիրը class-ի աշխատում է `User.prototype`-ում գեթթերներ և սեթթերներ ստեղծելով:

## Հաշվարկված անվանումներ [...]

Ահա `[...]` փակագծերի օգտագործմամբ հաշվարկված մեթոդի անվան օրինակ.

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

Նմանատիպ առանձնահատկությունները հեշտ է հիշել, քանի որ դրանք նման են բառացի օբյեկտներին։

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
