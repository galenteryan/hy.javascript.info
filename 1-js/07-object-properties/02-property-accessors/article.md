
# Հատկության ստացողներ (getters) և տեղադրողներ (setters)

Գոյություն ունեն օբյեկտի երկու տեսակի հատկություններ:

Առաջինը *տվյալային հատկություններ* (data properties) տեսակն է: Մենք արդեն գիտենք, թե ինչպես աշխատենք դրանց հետ: Բոլոր հատկությունները, որոնք մինչև հիմա օգտագործում էինք, տվյալային հատկություններ էին:

Հատկության երկրորդ տեսակը մի նոր բան է: Դա *մուտքային հատկություն* (accessor property) է. Դրանք ըստ իրենց էության ֆունկցիաներ են, որոնք կատարվում են արժեքի ստացման և տեղադրման ժամանակ, բայց արտաքին կոդից սովորական հատկությունների տեսք ունեն:

## Գեթթերներ և սեթթերներ

Մուտքային հատկությունները ներկայացված են «գեթթեր» (ստացող) և «սեթթեր» (տեղադրող) մեթոդներով: Օբյեկտում բառացիորեն դրանք նշվում են որպես `get` և `set`.

```js
let obj = {
  *!*get propName()*/!* {
    // գեթթեր, կոդը կատարվում է obj.propName ստացման ժամանակ
  },

  *!*set propName(value)*/!* {
    // սեթթեր, կոդը կատարվում է obj.propName = value տեղադրման ժամանակ
  }
};
```

Գեթթերն աշխատում է, երբ `obj.propName`-ն ընթերցվում է, իսկ սեթթերը՝ երբ նշանակվում է:

Օրինակի համար, մենք ունենք `user` օբյեկտ՝ `name` և `surname` հատկություններով.

```js
let user = {
  name: "John",
  surname: "Smith"
};
```

Այժմ ցանկանում ենք ավելացնել `fullName` հատկությունը, որը պետք է լինի `"John Smith"`: Իհարկե, մենք չենք ցանկանում կլոնավորել առկա ինֆորմացիան, այնպես որ կարող ենք իրագործել այն որպես մուտքային մեթոդ․

```js run
let user = {
  name: "John",
  surname: "Smith",

*!*
  get fullName() {
    return `${this.name} ${this.surname}`;
  }
*/!*
};

*!*
alert(user.fullName); // John Smith
*/!*
```

Մուտքային հատկությունն արտաքինից սովորական հատկության տեսք ունի: Դա է մուտքային հատկությունների գաղափարը: Մենք չենք *կանչում* `user.fullName`-ը որպես ֆունկցիա, այլ *ընթերցում* ենք այն սովորականի պես. գեթթերն աշխատում է «կուլիսների ետևում»:

Այս պահին `fullName`-ն ունի միայն գեթթեր: Եթե փորձենք արժեվորել `user.fullName=`, ապա տեղի կունենա սխալ․

```js run
let user = {
  get fullName() {
    return `...`;
  }
};

*!*
user.fullName = "Թեստ"; // Սխալ (հատկությունն ունի միայն գեթթեր)
*/!*
```

Եկեք շտկենք դա՝ ավելացնելով սեթթեր `user.fullName`-ի համար.

```js run
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

*!*
  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
*/!*
};

// set fullName կատարվում է տրված արժեքով
user.fullName = "Գասպար Գալենտերյան";

alert(user.name); // Գասպար
alert(user.surname); // Գալենտերյան
```

Արդյունքում մենք ունենք «վիրտուալ» հատկություն `fullName`: Այն հասանելի է ընթերցելու և արժեվորելու համար:

## Մուտքային տվյալների նկարագրիչներ

Մուտքային հատկությունների նկարագրիչները տարբերվում են տվյալային հատկությունների նկարագրիչներից:

Մուտքային հատկությունների համար չկա `value` կամ `writable`, բայց փոխարենը կան `get` և `set` ֆունկցիաներ:

Այսինքն, մուտքային հատկությունների նկարագրիչը կարող է ունենալ.

- **`get`** -- առանց արգումենտների ֆունկցիա, որն աշխատում է հատկության ընթերցման ժամանակ,
- **`set`** -- մեկ արգումենտով ֆունկցիա, որը կանչվում է հատկության արժեվորման ժամանակ,
- **`enumerable`** -- նույնը, ինչ տվյալային հատկությունների համար,
- **`configurable`** -- նույնը, ինչ տվյալային հատկությունների համար:

Օրինակի համար, `defineProperty`-ով `fullName` մուտքային հատկություն ստեղծելու համար, մենք կարող ենք փոխանցել նկարագրիչը `get`-ով և `set`-ով.

```js run
let user = {
  name: "John",
  surname: "Smith"
};

*!*
Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  }
*/!*
});

alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```

Նկատի ունեցեք, որ հատկությունը կարող է լինել մուտքային (ունի `get/set` մեթոդներ) կամ տվյալային (ունի `value`), բայց ոչ՝ երկուսն էլ միաժամանակ:

Եթե փորձենք կիրառել երկուսն էլ՝ `get` և `value` միևնույն նկարագրիչում, ապա տեղի կունենա սխալ.

```js run
*!*
// Error: Invalid property descriptor.
*/!*
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },

  value: 2
});
```

## Smarter getters/setters

Getters/setters can be used as wrappers over "real" property values to gain more control over operations with them.

For instance, if we want to forbid too short names for `user`, we can have a setter `name` and keep the value in a separate property `_name`:

```js run
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short, need at least 4 characters");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // Name is too short...
```

So, the name is stored in `_name` property, and the access is done via getter and setter.

Technically, external code is able to access the name directly by using `user._name`. But there is a widely known convention that properties starting with an underscore `"_"` are internal and should not be touched from outside the object.


## Using for compatibility

One of the great uses of accessors is that they allow to take control over a "regular" data property at any moment by replacing it with a getter and a setter and tweak its behavior.

Imagine we started implementing user objects using data properties `name` and `age`:

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

let john = new User("John", 25);

alert( john.age ); // 25
```

...But sooner or later, things may change. Instead of `age` we may decide to store `birthday`, because it's more precise and convenient:

```js
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
}

let john = new User("John", new Date(1992, 6, 1));
```

Now what to do with the old code that still uses `age` property?

We can try to find all such places and fix them, but that takes time and can be hard to do if that code is used by many other people. And besides, `age` is a nice thing to have in `user`, right?

Let's keep it.

Adding a getter for `age` solves the problem:

```js run no-beautify
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

*!*
  // age is calculated from the current date and birthday
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
*/!*
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday is available
alert( john.age );      // ...as well as the age
```

Now the old code works too and we've got a nice additional property.
