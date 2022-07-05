
# Ստատիկ հատկություններ և մեթոդներ

Մենք կարող ենք նաև մեթոդ նշանակել class-ին՝ որպես ամբողջություն: Նման մեթոդները կոչվում են *ստատիկ*։

Class-ի հայտարարագրում դրանք ամրագրված են `static` հիմնաբառով, այսպես.

```js run
class User {
*!*
  static staticMethod() {
*/!*
    alert(this === User);
  }
}

User.staticMethod(); // true
```

Սա իրականում անում է ճիշտ նույն բանը, ինչ անմիջապես հատկության նշանակումը.

```js run
class User { }

User.staticMethod = function() {
  alert(this === User);
};

User.staticMethod(); // true
```

`this`-ի արժեքը `User.staticMethod()` կանչի մեջ ինքնին `User` class-ի կոնստրուկտորն է («օբյեկտ կետից առաջ» կանոնով):

Սովորաբար ստատիկ մեթոդներն օգտագործվում են այնպիսի ֆունկցիաներ իրագործելու գամար, որոնք պատկանում են class-ին որպես ամբողջություն, բայց ոչ դրա որևէ կոնկրետ օբյեկտին:

Օրինակ՝ մենք ունենք `Article` օբյեկտներ և դրանց համեմատության համար անհրաժեշտ է ֆունկցիա:

Բնական լուծում կլինի, երբ ավելացնենք `Article.compare` ստատիկ մեթոդ.

```js run
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

*!*
  static compare(articleA, articleB) {
    return articleA.date - articleB.date;
  }
*/!*
}

// կիրառում
let articles = [
  new Article("HTML", new Date(2019, 1, 1)),
  new Article("CSS", new Date(2019, 0, 1)),
  new Article("JavaScript", new Date(2019, 11, 1))
];

*!*
articles.sort(Article.compare);
*/!*

alert( articles[0].title ); // CSS
```

Այստեղ `Article.compare` մեթոդը կանգնած է հոդվածների «վերևում»՝ որպես դրանք համեմատելու միջոց: Դա ոչ թե հոդվածի մեթոդ է, այլ ամբողջ class-ի:

Մեկ այլ օրինակ կլինի այսպես կոչված «գործարանային» մեթոդը:

Ենթադրենք, մեզ անհրաժեշտ են մի քանի եղանակներ հոդված ստեղծելու համար.

1. Ստեղծել տրված պարամետրերով (`title`, `date` և այլն):
2. Ստեղծել դատարկ հոդված այսօրվա ամսաթվով:
3. ...կամ էլ ինչ-որ այլ կերպ:

Առաջին տարբերակը կարող է իրականացվել կոնստրուկտորի կողմից։ Իսկ երկրորդ տարբերակը՝ մենք կարող ենք class-ի համար ստատիկ մեթոդ ստեղծել:

Ինչպիսին է `Article.createTodays()`-ն այստեղ․

```js run
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

*!*
  static createTodays() {
    // հիշեք, this = Article
    return new this("Այսօրվա ամփոփագիր", new Date());
  }
*/!*
}

let article = Article.createTodays();

alert( article.title ); // Այսօրվա ամփոփագիր
```

Այժմ արդեն, ամեն անգամ, երբ մենք պետք է ստեղծենք այսօրվա ամփոփագիր, մենք կարող ենք կանչել `Article.createTodays()`: Կրկին, դա ոչ թե հոդվածի մեթոդ է, այլ ամբողջ class-ի մեթոդ:

Ստատիկ մեթոդները օգտագործվում են նաև տվյալների բազայի հետ կապված class-ներում՝ տվյալների բազայից գրառումները որոնելու/պահելու/հեռացնելու համար, այսպես.

```js
// ենթադրելով, որ Article-ը հոդվածների կառավարման հատուկ class է
// id-ի միջոցով հոդվածը հեռացնելու ստատիկ մեթոդ.
Article.remove({id: 12345});
```

````warn header="Ստատիկ մեթոդները հասանելի չեն առանձին օբյեկտների համար"
Ստատիկ մեթոդները կարող են կանչվել class-ների, այլ ոչ թե առանձին օբյեկտների վրա:

Օր.՝ նման կոդը չի աշխատի.

```js
// ...
article.createTodays(); /// Error: article.createTodays is not a function
```
````

## Static properties

[recent browser=Chrome]

Static properties are also possible, they look like regular class properties, but prepended by `static`:

```js run
class Article {
  static publisher = "Ilya Kantor";
}

alert( Article.publisher ); // Ilya Kantor
```

That is the same as a direct assignment to `Article`:

```js
Article.publisher = "Ilya Kantor";
```

## Inheritance of static properties and methods [#statics-and-inheritance]

Static properties and methods are inherited.

For instance, `Animal.compare` and `Animal.planet` in the code below are inherited and accessible as `Rabbit.compare` and `Rabbit.planet`:

```js run
class Animal {
  static planet = "Earth";

  constructor(name, speed) {
    this.speed = speed;
    this.name = name;
  }

  run(speed = 0) {
    this.speed += speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }

*!*
  static compare(animalA, animalB) {
    return animalA.speed - animalB.speed;
  }
*/!*

}

// Inherit from Animal
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbits = [
  new Rabbit("White Rabbit", 10),
  new Rabbit("Black Rabbit", 5)
];

*!*
rabbits.sort(Rabbit.compare);
*/!*

rabbits[0].run(); // Black Rabbit runs with speed 5.

alert(Rabbit.planet); // Earth
```

Now when we call `Rabbit.compare`, the inherited `Animal.compare` will be called.

How does it work? Again, using prototypes. As you might have already guessed, `extends` gives `Rabbit` the `[[Prototype]]` reference to `Animal`.

![](animal-rabbit-static.svg)

So, `Rabbit extends Animal` creates two `[[Prototype]]` references:

1. `Rabbit` function prototypally inherits from `Animal` function.
2. `Rabbit.prototype` prototypally inherits from `Animal.prototype`.

As a result, inheritance works both for regular and static methods.

Here, let's check that by code:

```js run
class Animal {}
class Rabbit extends Animal {}

// for statics
alert(Rabbit.__proto__ === Animal); // true

// for regular methods
alert(Rabbit.prototype.__proto__ === Animal.prototype); // true
```

## Summary

Static methods are used for the functionality that belongs to the class "as a whole". It doesn't relate to a concrete class instance.

For example, a method for comparison `Article.compare(article1, article2)` or a factory method `Article.createTodays()`.

They are labeled by the word `static` in class declaration.

Static properties are used when we'd like to store class-level data, also not bound to an instance.

The syntax is:

```js
class MyClass {
  static property = ...;

  static method() {
    ...
  }
}
```

Technically, static declaration is the same as assigning to the class itself:

```js
MyClass.property = ...
MyClass.method = ...
```

Static properties and methods are inherited.

For `class B extends A` the prototype of the class `B` itself points to `A`: `B.[[Prototype]] = A`. So if a field is not found in `B`, the search continues in `A`.
