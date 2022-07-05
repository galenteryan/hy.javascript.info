Կարևորություն՝ 5

---

# Նմուշ ստեղծելու սխալ

Ահա կոդը, երբ `Rabbit`-ն ընդլայնվում է `Animal`-ից։

Ցավոք, `Rabbit`-ի օբյեկտները չեն կարող ստեղծվել։ Ի՞նչ խնդիր կա, շտկեք այն։
```js run
class Animal {

  constructor(name) {
    this.name = name;
  }

}

class Rabbit extends Animal {
  constructor(name) {  
    this.name = name;
    this.created = Date.now();
  }
}

*!*
let rabbit = new Rabbit("Սպիտակ Ճագար"); // Error: this is not defined
*/!*
alert(rabbit.name);
```
