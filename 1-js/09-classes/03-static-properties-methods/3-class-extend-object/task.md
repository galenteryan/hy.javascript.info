Կարևորություն՝ 3

---

# Class-ն ընդլայնո՞ւմ է օբյեկտը:

Ինչպես գիտենք, բոլոր օբյեկտները սովորաբար ժառանգում են `Object.prototype`-ից և ստանում հասանելիություն «ընդհանուր» օբյեկտի մեթոդներն, օր․՝ `hasOwnProperty` և այլն...

Օրինակ․

```js run
class Rabbit {
  constructor(name) {
    this.name = name;
  }
}

let rabbit = new Rabbit("Rab");

*!*
// hasOwnProperty մեթոդը Object.prototype-ից է
alert( rabbit.hasOwnProperty('name') ); // true
*/!*
```

Բայց, եթե մենք այն հստակ ձևակերպենք՝ `«class Rabbit extends Object»`, ապա արդյունքը կտարբերվի՞ արդյոք պարզ `«class Rabbit»`-ից:

Ո՞րն է տարբերությունը։

Ահա այսպիսի կոդի օրինակ (այն չի աշխատում․ ինչո՞ւ, ուղղեք այն):

```js
class Rabbit extends Object {
  constructor(name) {
    this.name = name;
  }
}

let rabbit = new Rabbit("Rab");

alert( rabbit.hasOwnProperty('name') ); // Error
```
