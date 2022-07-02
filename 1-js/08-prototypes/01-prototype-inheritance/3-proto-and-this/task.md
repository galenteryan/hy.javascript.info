Կարևորություն՝ 5

---

# Ո՞րտեղ է այն նշվում:

Մենք ունենք `rabbit`, որը ժառանգում է `animal`-ից:

Եթե կանչենք `rabbit.eat()`-ը, ապա ո՞ր օբյեկտն է ստանում `full` հատկությունը՝ `animal` թե՞ `rabbit`:

```js
let animal = {
  eat() {
    this.full = true;
  }
};

let rabbit = {
  __proto__: animal
};

rabbit.eat();
```
