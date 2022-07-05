Դա պայմանավորված է նրանով, որ երեխայի կոնստրուկտորը պետք է կանչի `super()`:

Ահա շտկված կոդը.

```js run
class Animal {

  constructor(name) {
    this.name = name;
  }

}

class Rabbit extends Animal {
  constructor(name) {  
    *!*
    super(name);
    */!*
    this.created = Date.now();
  }
}

*!*
let rabbit = new Rabbit("Սպիտակ Ճագար"); // այժմ նորմալ է
*/!*
alert(rabbit.name); // Սպիտակ Ճագար
```
