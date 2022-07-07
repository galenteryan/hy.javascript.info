Կարևորություն՝ 5

---

# Տարօրինակ instanceof

Ստորև բերված կոդում ինչո՞ւ է `instanceof`-ը վերադարձնում `true`: Մենք հեշտությամբ կարող ենք տեսնել, որ `a`-ն չի ստեղծվում `B()`-ով:

```js run
function A() {}
function B() {}

A.prototype = B.prototype = {};

let a = new A();

*!*
alert( a instanceof B ); // true
*/!*
```
