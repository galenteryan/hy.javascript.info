Կարևորություն՝ 5

---

# Աշխատանք նախատիպի հետ

Ահա կոդը, որը ստեղծում է զույգ օբյեկտներ, այնուհետև փոփոխում դրանք:

Ի՞նչ արժեքներ են ցուցադրվում ընթացքում:

```js
let animal = {
  jumps: null
};
let rabbit = {
  __proto__: animal,
  jumps: true
};

alert( rabbit.jumps ); // ? (1)

delete rabbit.jumps;

alert( rabbit.jumps ); // ? (2)

delete animal.jumps;

alert( rabbit.jumps ); // ? (3)
```

Պետք է լինի 3 պատասխան։
