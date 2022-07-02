Կարևորություն՝ 5

---

# Ինչո՞ւ են երկու համստերներն էլ կշտանում:

Ունենք երկու համստեր՝ `speedy` և `lazy`, որոնք ժառանգում են գլխավոր `hamster` օբյեկտից։ 

Երբ նրանցից մեկին կերակրում ենք, մյուսն էլ է կշտանում։ Ինչո՞ւ։ Ինչպե՞ս կարող ենք շտկել այն:

```js run
let hamster = {
  stomach: [],

  eat(food) {
    this.stomach.push(food);
  }
};

let speedy = {
  __proto__: hamster
};

let lazy = {
  __proto__: hamster
};

// Այս մեկը գտավ ուտելիք
speedy.eat("խնձոր");
alert( speedy.stomach ); // խնձոր

// Այս մեկն էլ ունի, ինչո՞ւ։ ՈՒղղեք խնդրում եմ:
alert( lazy.stomach ); // խնձոր
```

