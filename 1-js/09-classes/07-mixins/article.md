# Խառնուրդներ (mixins)

JavaScript-ում կարող ենք ժառանգել միայն մեկ օբյեկտից: Օբյեկտի համար կարող է լինել միայն մեկ `[[Prototype]]`: Եվ class-ը կարող է ընդլայնել միայն մեկ class:

Բայց, երբեմն դա սահմանափակ է թվում: Օրինակ՝ մենք ունենք `StreetSweeper` և `Bicycle` class-ներ, և ցանկանում ենք ստեղծել դրանց խառնուրդը՝ `StreetSweepingBicycle`:

Կամ, ունենք `User` class և `EventEmitter` class, որն իրագործում է իրադարձությունների ստեղծումը, և մեզ անհրաժեշտ է ավելացնել `EventEmitter`-ի ֆունկցիոնալը `User`-ում, որպեսզի մեր օգտվողները կարողանան ստեղծել իրադարձություններ:

Կա մի հասկացություն, որը կարող է օգնել մեզ այստեղ, դա կոչվում է «խառնուրդներ» (mixins):

Ինչպես սահմանված է Վիքիպեդիայում, [mixin](https://en.wikipedia.org/wiki/Mixin)-ը class է, որը պարունակում է մեթոդներ, որոնք կարող են օգտագործվել այլ class-ների կողմից՝ առանց դրանից ժառանգելու անհրաժեշտության։

Այլ կերպ ասած, *mixin*-ը տրամադրում է մեթոդներ, որոնք իրականացնում են որոշակի վարքագիծ, բայց մենք այն չենք օգտագործում որպես այդպիսին, այլ օգտագործում ենք դա՝ իր վարքագիծը այլ class-ներում ավելացնելու համար։

## Խառնուրդի օրինակ

JavaScript-ում խառնուրդիի ներդրման ամենապարզ ձևը օգտակար մեթոդներով օբյեկտ պատրաստելն է, որպեսզի հեշտությամբ կարողանանք դրանք միավորել ցանկացած class-ի նախատիպի մեջ:

Օրինակի համար, այստեղ `sayHiMixin` խառնուրդիը օգտագործվում է `User`-ի համար ինչ-որ «խոսք» ավելացնելու նպատակով.

```js run
*!*
// խառնուրդ
*/!*
let sayHiMixin = {
  sayHi() {
    alert(`Ողջույն ${this.name}`);
  },
  sayBye() {
    alert(`Ցտեսություն ${this.name}`);
  }
};

*!*
// օգտագործում՝
*/!*
class User {
  constructor(name) {
    this.name = name;
  }
}

// մեթոդների պատճենում
Object.assign(User.prototype, sayHiMixin);

// այժմ User-ը կարող է ասել ողջույն
new User("Dude").sayHi(); // Ողջույն Dude
```

There's no inheritance, but a simple method copying. So `User` may inherit from another class and also include the mixin to "mix-in" the additional methods, like this:
Չկա ժառանգություն, բայց կա պատճենահանման պարզ մեթոդ: Այսպիսով, `User`-ը կարող է ժառանգել մեկ այլ class-ից, ինչպես նաև ներառել խառնուրդը՝ լրացուցիչ մեթոդները «խառնելու» համար, այսպես.

```js
class User extends Person {
  // ...
}

Object.assign(User.prototype, sayHiMixin);
```

Խառնուրդները կարող են օգտվել ժառանգությունից իրենց ներսում:

Օրինակ՝ այստեղ `sayHiMixin`-ը ժառանգում է `sayMixin`-ից․

```js run
let sayMixin = {
  say(phrase) {
    alert(phrase);
  }
};

let sayHiMixin = {
  __proto__: sayMixin, // (կամ այստեղ նախատիպը տեղադրելու համար կարող ենք օգտագործել Object.setPrototypeOf)

  sayHi() {
    *!*
    // կանչում ենք ծնողի մեթոդը
    */!*
    super.say(`Ողջույն ${this.name}`); // (*)
  },
  sayBye() {
    super.say(`Ցտեսություն ${this.name}`); // (*)
  }
};

class User {
  constructor(name) {
    this.name = name;
  }
}

// պատճենում ենք մեթոդները
Object.assign(User.prototype, sayHiMixin);

// այժմ User-ը կարող է ասել ողջույն
new User("Dude").sayHi(); // Ողջույն Dude
```

Նկատի ունեցեք, որ `super.say()`-ից `super.say()` ծնող մեթոդի կանչը (`(*)` պիտակավորված տողերում) որոնում է մեթոդը այդ խառնուրդի նախատիպում, այլ ոչ թե class-ի նախատիպում:

Ահա գծապատկերը (տեսեք աջ մասը).

![](mixin-inheritance.svg)

Դա պայմանավորված է նրանով, որ `sayHi` և `sayBye` մեթոդներն ի սկզբանե ստեղծվել են `sayHiMixin`-ում: Այսպիսով, չնայած դրանք պատճենվել են, բայց դրանց `[[HomeObject]]` ներքին հատկությունը հղում է անում `sayHiMixin`-ին, ինչպես ցուցադրված է վերևի նկարում:

Քանի որ `super`-ը `[[HomeObject]].[[Prototype]]`-ում փնտրում է ծնողի մեթոդները, ապա դա նշանակում է, որ այն փնտրում է `sayHiMixin.[[Prototype]]`-ում, այլ ոչ թե `User.[[Prototype]]`-ում:

## EventMixin

Now let's make a mixin for real life.

An important feature of many browser objects (for instance) is that they can generate events. Events are a great way to "broadcast information" to anyone who wants it. So let's make a mixin that allows us to easily add event-related functions to any class/object.

- The mixin will provide a method `.trigger(name, [...data])` to "generate an event" when something important happens to it. The `name` argument is a name of the event, optionally followed by additional arguments with event data.
- Also the method `.on(name, handler)` that adds `handler` function as the listener to events with the given name. It will be called when an event with the given `name` triggers, and get the arguments from the `.trigger` call.
- ...And the method `.off(name, handler)` that removes the `handler` listener.

After adding the mixin, an object `user` will be able to generate an event `"login"` when the visitor logs in. And another object, say, `calendar` may want to listen for such events to load the calendar for the logged-in person.

Or, a `menu` can generate the event `"select"` when a menu item is selected, and other objects may assign handlers to react on that event. And so on.

Here's the code:

```js run
let eventMixin = {
  /**
   * Subscribe to event, usage:
   *  menu.on('select', function(item) { ... }
  */
  on(eventName, handler) {
    if (!this._eventHandlers) this._eventHandlers = {};
    if (!this._eventHandlers[eventName]) {
      this._eventHandlers[eventName] = [];
    }
    this._eventHandlers[eventName].push(handler);
  },

  /**
   * Cancel the subscription, usage:
   *  menu.off('select', handler)
   */
  off(eventName, handler) {
    let handlers = this._eventHandlers?.[eventName];
    if (!handlers) return;
    for (let i = 0; i < handlers.length; i++) {
      if (handlers[i] === handler) {
        handlers.splice(i--, 1);
      }
    }
  },

  /**
   * Generate an event with the given name and data
   *  this.trigger('select', data1, data2);
   */
  trigger(eventName, ...args) {
    if (!this._eventHandlers?.[eventName]) {
      return; // no handlers for that event name
    }

    // call the handlers
    this._eventHandlers[eventName].forEach(handler => handler.apply(this, args));
  }
};
```


- `.on(eventName, handler)` -- assigns function `handler` to run when the event with that name occurs. Technically, there's an `_eventHandlers` property that stores an array of handlers for each event name, and it just adds it to the list.
- `.off(eventName, handler)` -- removes the function from the handlers list.
- `.trigger(eventName, ...args)` -- generates the event: all handlers from `_eventHandlers[eventName]` are called, with a list of arguments `...args`.

Usage:

```js run
// Make a class
class Menu {
  choose(value) {
    this.trigger("select", value);
  }
}
// Add the mixin with event-related methods
Object.assign(Menu.prototype, eventMixin);

let menu = new Menu();

// add a handler, to be called on selection:
*!*
menu.on("select", value => alert(`Value selected: ${value}`));
*/!*

// triggers the event => the handler above runs and shows:
// Value selected: 123
menu.choose("123");
```

Now, if we'd like any code to react to a menu selection, we can listen for it with `menu.on(...)`.

And `eventMixin` mixin makes it easy to add such behavior to as many classes as we'd like, without interfering with the inheritance chain.

## Summary

*Mixin* -- is a generic object-oriented programming term: a class that contains methods for other classes.

Some other languages allow multiple inheritance. JavaScript does not support multiple inheritance, but mixins can be implemented by copying methods into prototype.

We can use mixins as a way to augment a class by adding multiple behaviors, like event-handling as we have seen above.

Mixins may become a point of conflict if they accidentally overwrite existing class methods. So generally one should think well about the naming methods of a mixin, to minimize the probability of that happening.
