Նախ, եկեք տեսնենք, թե ինչու վերջին կոդը չի աշխատում:

Պատճառն ակնհայտ է դառնում, եթե փորձենք գործարկել այն։ Ժառանգող class-ի կոնստրուկտորը պետք է կանչի `super()`-ին: Հակառակ դեպքում `«this»`-ը չի «սահմանվի»:

Այսպիսով, ահա ուղղումը.

```js run
class Rabbit extends Object {
  constructor(name) {
*!*
    super(); // ժառանգելիս անհրաժեշտ է կանչել ծնող կոնստրուկտորը
*/!*
    this.name = name;
  }
}

let rabbit = new Rabbit("Rab");

alert( rabbit.hasOwnProperty('name') ); // true
```

Բայց դա դեռ ամենը չէ։

Նույնիսկ ուղղումից հետո, դեռևս կարևոր տարբերություն կա `class Rabbit extends Object`-ի և `class Rabbit`-ի միջև:

Ինչպես գիտենք, «extends» շարահյուսությունը ստեղծում է երկու նախատիպ.

1. Կոնստրուկտոր ֆունկցիաների `«prototype»`-երի միջև (մեթոդների համար):
2. Անմիւապես կոնստրուկտոր ֆունկցիաների միջև (ստատիկ մեթոդների համար):

`class Rabbit extends Object`-ի դեպքում դա նշանակում է.

```js run
class Rabbit extends Object {}

alert( Rabbit.prototype.__proto__ === Object.prototype ); // (1) true
alert( Rabbit.__proto__ === Object ); // (2) true
```

Այսպիսով, `Rabbit`-ն այժմ `Rabbit`-ի միջոցով ապահովում է հասանելիություն `Object`-ի ստատիկ մեթոդներին, այսպես.

```js run
class Rabbit extends Object {}

*!*
// սովորաբար մենք կանչում ենք Object.getOwnPropertyNames
alert ( Rabbit.getOwnPropertyNames({a: 1, b: 2})); // a,b
*/!*
```

Բայց եթե մենք չունենք `extends Object`, ապա `Rabbit.__proto__`-ն սահմանված չէ որպես `Object`:

Ահա ցուցադրումը.

```js run
class Rabbit {}

alert( Rabbit.prototype.__proto__ === Object.prototype ); // (1) true
alert( Rabbit.__proto__ === Object ); // (2) false (!)
alert( Rabbit.__proto__ === Function.prototype ); // ինչպես ցանկացած ֆունկցիա կանխադրված կերպով

*!*
// սխալ, Rabbit-ում նման ֆունկցիա չկա
alert ( Rabbit.getOwnPropertyNames({a: 1, b: 2})); // Error
*/!*
```

Այսպիսով, `Rabbit`-ն այդ դեպքում հասանելիություն չի ապահովում `Object`-ի ստատիկ մեթոդներին:

Ի դեպ, `Function.prototype`-ն ունի նաև «ընդհանուր» ֆունկցիայի մեթոդներ, ինչպիսիք են՝ `call`, `bind` և այլն․․․ Դրանք ի վերջո երկու դեպքում էլ հասանելի են, քանի որ ներկառուցված `Object`-ի կոնստրուկտորի համար՝ `Object.__proto__ === Function.prototype`:

Ահա պատկերը.

![](rabbit-extends-object.svg)

Կարճ ասած՝ երկու տարբերություն կա.

| class Rabbit | class Rabbit extends Object  |
|--------------|------------------------------|
| --             | անհրաժեշտ է կոնստրուկտորում կանչել `super()`-ին |
| `Rabbit.__proto__ === Function.prototype` | `Rabbit.__proto__ === Object` |
