Մենք կարող ենք նման մոտեցում կիրառել, եթե վստահ լինենք, որ `«constructor»` հատկությունը ճշգրիտ արժեք ունի:

Օրինակ՝ եթե մենք չդիպչենք կանխադրված `«prototype»`-ին, ապա այս կոդը հաստատ կաշխատի.

```js run
function User(name) {
  this.name = name;
}

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // Pete (աշխատեց)
```

Այն աշխատեց, քանի որ `User.prototype.constructor == User`:

..Բայց եթե ինչ-որ մեկը, այսպես ասած, վերագրի `User.prototype`-ը և մոռանա վերստեղծել `constructor`-ը՝ հղում անելով `User`-ին, ապա այն կձախողվի:

Օրինակ․

```js run
function User(name) {
  this.name = name;
}
*!*
User.prototype = {}; // (*)
*/!*

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // undefined
```

Ինչո՞ւ է `user2.name`-ը `undefined`։

Ահա, թե ինչպես է աշխատում `new user.constructor('Pete')`-ը․

1. Նախ, այն `user`-ում փնտրում է `constructor`: Ոչինչ չկա։
2. Այնուհետև այն հետևում է նախատիպի շղթային: `user`-ի նախատիպը `User.prototype`-ն է, իսկ այն նաև `constructor` չունի (քանի որ մենք «մոռացել» ենք այն ճիշտ կարգավորել):
3. Շղթայով ավելի վեր բարձրանալով նկատվում է, որ `User.prototype`-ը պարզ օբյեկտ է, իսկ դրա նախատիպը ներկառուցված `Object.prototype`-ն է:
4. Վերջապես, ներկառուցված `Object.prototype`-ի համար կա ներկառուցված `Object.prototype.constructor == Object`: Հետևաբար դա է օգտագործվում:

Ի վերջո, ամենավերջում մենք ունենք `let user2 = new Object('Pete')`:

Հավանաբար, դա այն չէ, ինչ մեզ պետք է։ Մեզ անհրաժեշտ է ստեղծել `new User`, այլ ոչ թե `new Object`: Դա բացակայող `constructor`-ի հետևանքն է:

(Պարզապես այն դեպքում, երբ դուք հետաքրքրված եք, `new Object(...)`-ի կանչն իր արգումենտը փոխակերպում է օբյեկտի: Դա տեսական երևույթ է, գործնականում ոչ ոք արժեքով չի կանչում `new Object`, և, առհասարակ, մենք օբյեկտներ ստեղծելու համար ընդհանրապես չենք օգտագործում `new Object`):
