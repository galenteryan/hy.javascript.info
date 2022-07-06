
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

## Ստատիկ հատկություններ

[recent browser=Chrome]

Հնարավոր է ստեղծել նաև ստատիկ հատկություններ, որոնք նման են class-ի սովորական հատկություններին, բայց նշված են `static`-ով.

```js run
class Article {
  static publisher = "Ilya Kantor";
}

alert( Article.publisher ); // Ilya Kantor
```

Դա նույնն է, ինչ `Article`-ին ուղղակիորեն նշանակումը.

```js
Article.publisher = "Ilya Kantor";
```

## Ստատիկ հատկությունների և մեթոդների ժառանգում [#statics-and-inheritance]

Ստատիկ հատկությունները և մեթոդները ժառանգվում են:

Օրինակ՝ `Animal.compare`-ը և `Animal.planet`-ը ստորև կոդում ժառանգվում են և հասանելի են դառնում որպես `Rabbit.compare` և `Rabbit.planet`․

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

Այժմ, երբ մենք կանչում ենք `Rabbit.compare`-ը, կկանչվի ժառանգած `Animal.compare`-ը:

Ինչպե՞ս է դա աշխատում։ Կրկին՝ օգտագործելով նախատիպերը: Ինչպես արդեն կռահեցիք, `extends`-ը `Rabbit`-ին տալիս է `[[Prototype]]` հղումը դեպի `Animal`:

![](animal-rabbit-static.svg)

Այսպիսով, `Rabbit extends Animal`-ը ստեղծում է երկու `[[Prototype]]` հղում.

1. `Rabbit` ֆունկցիան նախատիպով ժառանգում է `Animal` ֆունկցիայից:
2. `Rabbit.prototype`-ը նախատիպով ժառանգում է `Animal.prototype`-ից:

Արդյունքում ժառանգությունը գործում է ինչպես կանոնավոր, այնպես էլ ստատիկ մեթոդների դեպքում:

Ահա, եկեք ստուգենք դա կոդով.

```js run
class Animal {}
class Rabbit extends Animal {}

// for statics
alert(Rabbit.__proto__ === Animal); // true

// for regular methods
alert(Rabbit.prototype.__proto__ === Animal.prototype); // true
```

## Ամփոփում

Ստատիկ մեթոդներն օգտագործվում են այն ֆունկցիոնալի համար, որն «ամբողջությամբ» պատկանում է class-ին: Դա չի վերաբերում կոնկրետ class-ի օրինակին:

Օրինակ՝ `Article.compare(article1, article2)` համեմատության մեթոդը կամ `Article.createTodays()` գործարանային մեթոդը:

Class-ի հայտարարագրում դրանք պիտակավորված են `static` բառով:

Ստատիկ հատկություններն օգտագործվում են, երբ մենք ցանկանում ենք պահել class-ի մակարդակի տվյալներ, ինչպես նաև չկապված որևէ նմուշի հետ:

Շարահյուսությունը հետևյալն է.

```js
class MyClass {
  static property = ...;

  static method() {
    ...
  }
}
```

Տեխնիկապես ստատիկ հայտարարությունը նույնն է, ինչ անմիջապես class-ին վերագրելը.

```js
MyClass.property = ...
MyClass.method = ...
```

Ստատիկ հատկություններն ու մեթոդները ժառանգվում են:

`class B extends A`-ի համար `B` class-ի նախատիպն ինքնին հղում է անում `A`-ին. `B.[[Prototype]] = A`: Այսպիսով, եթե դաշտը չի գտնվել `B`-ում, որոնումը շարունակվում է `A`-ում:
