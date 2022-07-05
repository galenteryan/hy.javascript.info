
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

Որպեսզի `Rabbit` կոնստրուկտորն աշխատի, `this`-ն օգտագործելուց առաջ այն պետք է կանչի `super()`-ը, ինչպես այստեղ.

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

### Գերակայող class-ի դաշտերը. բարդ նշում

```warn header="Ընդլայնված նշում"
Այս նշումը ենթադրում է, որ դուք որոշակի փորձ ունեք class-ների հետ, գուցե այլ ծրագրավորման լեզուներով:

Այն ավելի լավ պատկերացում է ձևավորում լեզվի մասին, նաև բացատրում է վարքագիծը, որը կարող է սխալների աղբյուր հանդիսանալ (բայց ոչ շատ հաճախ):

Եթե դժվարանում եք հասկանալ, պարզապես շարժվեք առաջ, շարունակեք ընթերցել և որոշ ժամանակ անց կրկին անդրադարձեք այս թեմային:
```

Մենք կարող ենք անտեսել ոչ միայն մեթոդները, այլև class-ի դաշտերը:

Չնայած, կա մի բարդ վարքագիծ, երբ մենք մուտք ենք գործում գերակայող դաշտ ծնող կոնստրուկտորում, դա բոլորովին տարբերվում է մի շարք ծրագրավորման լեզուներից:

Դիտարկենք այս օրինակը.

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

Այստեղ `Rabbit` class-ը ընդլայնվում է `Animal`-ից և անտեսում է `name` դաշտն իր արժեքով:

`Rabbit`-ում սեփական կոնստրուկտոր չկա, ուստի կանչվում է `Animal`-ի կոնստրուկտորը:

Հետաքրքիրն այն է, որ երկու դեպքում էլ՝ `new Animal()` և `new Rabbit()`, `alert`-ը `(*)` տողում ցույց է տալիս `animal`:

**Այլ կերպ ասած, ծնող կոնստրուկտորը միշտ օգտագործում է իր դաշտի արժեքը, այլ ոչ թե գերակայվողը:**

Ի՞նչ տարօրինակ բան կա դրա մեջ:

Եթե դեռ պարզ չէ, խնդրում ենք համեմատել մեթոդների հետ։

Ահա նույն կոդը, բայց `this.name` դաշտի փոխարեն, մենք կանչում ենք `this.showName()` մեթոդը.

```js run
class Animal {
  showName() {  // «this.name = 'կենդանի'»-ի փոխարեն
    alert('կենդանի');
  }

  constructor() {
    this.showName(); // «alert(this.name);»-ի փոխարեն
  }
}

class Rabbit extends Animal {
  showName() {
    alert('ճագար');
  }
}

new Animal(); // կենդանի
*!*
new Rabbit(); // ճագար
*/!*
```

Նկատի ունեցեք. այժմ արդյունքն այլ է:

Եվ դա այն է, ինչ մենք բնականաբար ակնկալում էինք: Երբ ծնող կոնստրուկտորը կանչվում է ածանցյալ class-ում, այն օգտագործում է գերակայող մեթոդը:

...Բայց class-ի դաշտերի համար այդպես չէ։ Ինչպես նշվեց, ծնող կոնստրուկտորը միշտ օգտագործում է ծնող դաշտը:

Ինչո՞ւ կա տարբերություն:

Դե, պատճառը դաշտի սկզբնավորման կարգն է։ Class-ի դաշտը սկզբնավորվում է.
- Նախքան կոնստրուկտորը՝ հիմնական class-ի համար (որը ոչինչ չի ընդլայնում),
- Անմիջապես `super()`-ից հետո՝ ածանցյալ class-ի համար:

Մեր դեպքում `Rabbit`-ը ածանցյալ class-ն է: Դրանում `constructor()` չկա: Ինչպես նախկինում նշվեց, դա նույնն է, ինչ եթե դատարկ կոնստրուկտոր լիներ միայն `super(...args)`-ով:

Այսպիսով, `new Rabbit()`-ը կանչում է `super()`-ին՝ գործարկելով ծնող կոնստրուկտորը, և (ըստ ածանցյալ class-ների կանոնի) միայն այն բանից հետո, երբ իր class-ի դաշտերը սկզբնավորվում են: Ծնող կոնստրուկտորի գործարկման պահին `Rabbit`-ում class-ի դաշտեր դեռ չկան, այդ իսկ պատճառով օգտագործվում են `Animal`-ի դաշտերը։

Դաշտերի և մեթոդների միջև այս նուրբ տարբերությունը հատկանշական է JavaScript-ին:

Բարեբախտաբար, այս վարքագիծը բացահայտվում է միայն այն դեպքում, երբ գերակայող դաշտն օգտագործվում է ծնող կոնստրուկտորում: Այնուհետև գուցե դժվար լինի հասկանալ, թե ինչ է կատարվում, ուստի դա բացատրվում է այստեղ:

Եթե դա խնդիր է առաջացնում, կարելի է ուղղել այն՝ դաշտերի փոխարեն օգտագործելով մեթոդներ կամ գեթթերներ/սեթթերներ:

## Super. ներքին կառուցվածք, [[HomeObject]]

```warn header="Ընդլայնված տեղեկատվություն"
Եթե դուք առաջին անգամ եք ընթերցում ձեռնարկը, այս բաժինը կարող եք բաց թողնել:

Սա ժառանգականության և `super`-ի հետևում գտնվող ներքին մեխանիզմների մասին է:
```

Եկեք մի փոքր խորանանք `super`-ի ներքո։ Հետաքրքիր բաներ կտեսնենք ճանապարհին:

Առաջին հերթին, այն ամենի հիմքով, ինչ սովորել ենք մինչ այժմ, անհնար է, որ `super`-ն ընդհանրապես աշխատի:

Այո, իսկապես, եկեք ինքներս մեզ հարցնենք՝ տեխնիկապես ինչպե՞ս այն պետք է աշխատի: Երբ գործարկվում է օբյեկտի մեթոդը, այն ստանում է ընթացիկ օբյեկտը որպես `this`: Եթե մենք կանչենք `super.method()`, ապա շարժիչը պետք է ստանա `method`-ն ընթացիկ օբյեկտի նախատիպից: Բայց ինչպե՞ս։

Առաջադրանքը կարող է պարզ թվալ, բայց դա այդպես չէ: Շարժիչը գիտի `this` ընթացիկ օբյեկտը, ուստի այն կարող է ստանալ ծնող `method`-ը որպես `this.__proto__.method`: Ցավոք սրտի, նման «միամիտ» լուծումը չէ, որ աշխատում է իրականում։

Եկեք խնդիրն ավելի պարզ ձևով պատկերացնելու նպատակով դիտարկենք առանց class-ների, այլ պարզ օբյեկտների օրինակով:

Եթե չեք ցանկանում իմանալ մանրամասները, ապա կարող եք բաց թողնել այս մասը և անցնել `[[HomeObject]]` ենթաբաժնին: Դա չի վնասի: Կամ ընթերցեք, եթե հետաքրքիր է ամեն ինչ խորությամբ հասկանալը:

Ստորև բերված օրինակում՝ `rabbit.__proto__ = animal`: Հիմա եկեք փորձենք. `rabbit.eat()`-ում կկանչենք `animal.eat()`-ը՝ օգտագործելով `this.__proto__`-ն:

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

`(*)` տողում մենք վերցնում ենք `eat`-ը նախատիպից (`animal`) և այն կանչում ենք ընթացիկ օբյեկտի համատեքստում: Նկատի ունեցեք, որ `.call(this)`-ը կարևոր է այստեղ, քանի որ պարզ `this.__proto__.eat()`-ը կգործարկի ծնողի `eat`-ը նախատիպի համատեքստում, այլ ոչ թե ընթացիկ օբյեկտի:

Եվ վերը նշված կոդում այն իրականում աշխատում է այնպես, ինչպես նախատեսված է. մենք ունենք ճշգրիտ `alert`:

Հիմա շղթային ավելացնենք ևս մեկ օբյեկտ։ Մենք կտեսնենք, թե ինչպես է ամեն ինչ կոտրվում.

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

Կոդն այլևս չի աշխատում: Մենք կարող ենք տեսնել սխալ, երբ փորձենք կանչել `longEar.eat()`-ը:

Հավանաբար դա այնքան էլ ակնհայտ չէ, բայց եթե մենք հետևենք `longEar.eat()` կանչին, ապա մենք կարող ենք նկատել, թե ինչու: Երկու տողերում էլ՝ `(*)` և `(**)`, `this`-ի արժեքը ընթացիկ օբյեկտն է (`longEar`): Դա կարևոր է. օբյեկտների բոլոր մեթոդները ստանում են ընթացիկ օբյեկտը որպես `this`, այլ ոչ թե նախատիպ կամ մեկ այլ բան:

Այսպիսով, `(*)` և `(**)` տողերում էլ `this.__proto__`-ի արժեքը նույնն է՝ `rabbit`: Նրանք երկուսն էլ կանչում են `rabbit.eat`-ը՝ չբարձրանալով շղթան անվերջ ցիկլով:

Ահա պատկերը, թե ինչ է տեղի ունենում.

![](this-super-loop.svg)

1. Inside `longEar.eat()`, the line `(**)` calls `rabbit.eat` providing it with `this=longEar`. `longEar.eat()`-ի ներսում `(**)` տողը կանչում է `rabbit.eat`-ին՝ տրամադրելով `this=longEar`-ը:
    ```js
    // longEar.eat()-ի ներսում մենք ունենք՝ this = longEar
    this.__proto__.eat.call(this) // (**)
    // ստացվում է
    longEar.__proto__.eat.call(this)
    // դա նույնն է, ինչ
    rabbit.eat.call(this);
    ```
2. Այնուհետև `rabbit.eat`-ի `(*)` տողում մեզ անհրաժեշտ էր կանչը փոխանցել շղթայով ավելի բարձր, բայց `this=longEar`, այնպես որ `this.__proto__.eat`-ը կրկին `rabbit.eat` է։

    ```js
    // rabbit.eat()-ի ներսում մենք նույնպես ունենք՝ this = longEar
    this.__proto__.eat.call(this) // (*)
    // ստացվում է
    longEar.__proto__.eat.call(this)
    // կամ (կրկին)
    rabbit.eat.call(this);
    ```

3. ...Այսպիսով, `rabbit.eat`-ն իրեն կանչում է անվերջ ցիկլում, քանի որ այն չի կարող ավելի բարձրանալ:

Խնդիրը չի կարող լուծվել միայն `this` օգտագործելով:

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
