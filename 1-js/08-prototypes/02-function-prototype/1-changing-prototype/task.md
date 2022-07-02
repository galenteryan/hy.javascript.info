Կարևորություն՝ 5

---

# «Նախատիպի» փոփոխություն

Ստորև գտնվող կոդում մենք ստեղծում ենք `new Rabbit`, այնուհետև փորձում ենք փոփոխել դրա նախատիպը:

Սկզբում մենք ունենք այս կոդը.

```js run
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

alert( rabbit.eats ); // true
```


1. Ավելացրեցինք ևս մեկ տող (ընդգծված է): Ի՞նչ ցույց կտա `alert`-ը հիմա։

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    Rabbit.prototype = {};
    */!*

    alert( rabbit.eats ); // ?
    ```

2. ...Իսկ եթե կոդը այսպիսի՞ն է (մեկ տող փոխարինված է).

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    Rabbit.prototype.eats = false;
    */!*

    alert( rabbit.eats ); // ?
    ```

3. Իսկ այսպե՞ս (մեկ տող փոխարինված է):

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    delete rabbit.eats;
    */!*

    alert( rabbit.eats ); // ?
    ```

4. Վերջին տարբերակը.

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    delete Rabbit.prototype.eats;
    */!*

    alert( rabbit.eats ); // ?
    ```
