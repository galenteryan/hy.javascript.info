# Class-ի ստուգում․ «instanceof»

`instanceof` օպերատորը թույլ է տալիս ստուգել՝ արդյո՞ք օբյեկտը պատկանում է որևէ class-ին: Այն նաև հաշվի է առնում ժառանգությունը:

Նման ստուգումը շատ դեպքերում կարող է անհրաժեշտ լինել: Օրինակ՝ այն կարող է օգտագործվել *պոլիմորֆ* (polymorphic) ֆունկցիա կառուցելու համար, որը տարբեր ձևերով է վերաբերվում արգումենտներին՝ կախված դրանց տեսակից:

## Instanceof օպերատորը [#ref-instanceof]

Շարահյուսությունը հետևյալն է.
```js
obj instanceof Class
```

Այն վերադարձնում է `true`, եթե `obj`-ն պատկանում է `Class`-ին կամ նրանից ժառանգած class-ին:

Օրինակ՝

```js run
class Rabbit {}
let rabbit = new Rabbit();

// արդյո՞ք դա Rabbit class-ի օբյեկտ է
*!*
alert( rabbit instanceof Rabbit ); // true
*/!*
```

Այն նաև աշխատում է կոնստրուկտոր ֆունկցիաների հետ.

```js run
*!*
// instead of class
function Rabbit() {}
*/!*

alert( new Rabbit() instanceof Rabbit ); // true
```

...Նաև ներկառուցված class-ների հետ, ինչպիսին է `Array`-ը․

```js run
let arr = [1, 2, 3];
alert( arr instanceof Array ); // true
alert( arr instanceof Object ); // true
```

Նկատի ունեցեք, որ `arr`-ը նույնպես պատկանում է `Object` class-ին: Դա պայմանավորված է նրանով, որ `Array`-ը նախատիպով ժառանգում է `Object`-ից:

Սովորաբար, ստուգելու համար `instanceof`-ն ուսումնասիրում է նախատիպային շղթան: Մենք կարող ենք նաև հատուկ տրամաբանություն սահմանել `Symbol.hasInstance` ստատիկ մեթոդում:

`obj instanceof Class`-ի ալգորիթմն աշխատում է մոտավորապես հետևյալ կերպ.

1. Եթե կա `Symbol.hasInstance` ստատիկ մեթոդ, ապա պարզապես կանչել այն՝ `Class[Symbol.hasInstance](obj)`: Այն պետք է վերադարձնի կամ `true` կամ `false`, վերջ: Ահա, թե ինչպես կարող ենք կարգավորել `instanceof`-ի պահելաձևը:

    Օրինակ՝

    ```js run
    // ստեղծենք instanceOf-ի օրինակ, որը ենթադրում է, որ
    // canEat հատկություն ունեցող ցանկացած բան animal է
    class Animal {
      static [Symbol.hasInstance](obj) {
        if (obj.canEat) return true;
      }
    }

    let obj = { canEat: true };

    alert(obj instanceof Animal); // true՝ Animal[Symbol.hasInstance](obj)-ն է կանչվել
    ```

2. Most classes do not have `Symbol.hasInstance`. In that case, the standard logic is used: `obj instanceOf Class` checks whether `Class.prototype` is equal to one of the prototypes in the `obj` prototype chain.

    In other words, compare one after another:
    ```js
    obj.__proto__ === Class.prototype?
    obj.__proto__.__proto__ === Class.prototype?
    obj.__proto__.__proto__.__proto__ === Class.prototype?
    ...
    // if any answer is true, return true
    // otherwise, if we reached the end of the chain, return false
    ```

    In the example above `rabbit.__proto__ === Rabbit.prototype`, so that gives the answer immediately.

    In the case of an inheritance, the match will be at the second step:

    ```js run
    class Animal {}
    class Rabbit extends Animal {}

    let rabbit = new Rabbit();
    *!*
    alert(rabbit instanceof Animal); // true
    */!*

    // rabbit.__proto__ === Animal.prototype (no match)
    *!*
    // rabbit.__proto__.__proto__ === Animal.prototype (match!)
    */!*
    ```

Here's the illustration of what `rabbit instanceof Animal` compares with `Animal.prototype`:

![](instanceof.svg)

By the way, there's also a method [objA.isPrototypeOf(objB)](mdn:js/object/isPrototypeOf), that returns `true` if `objA` is somewhere in the chain of prototypes for `objB`. So the test of `obj instanceof Class` can be rephrased as `Class.prototype.isPrototypeOf(obj)`.

It's funny, but the `Class` constructor itself does not participate in the check! Only the chain of prototypes and `Class.prototype` matters.

That can lead to interesting consequences when a `prototype` property is changed after the object is created.

Like here:

```js run
function Rabbit() {}
let rabbit = new Rabbit();

// changed the prototype
Rabbit.prototype = {};

// ...not a rabbit any more!
*!*
alert( rabbit instanceof Rabbit ); // false
*/!*
```

## Bonus: Object.prototype.toString for the type

We already know that plain objects are converted to string as `[object Object]`:

```js run
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // the same
```

That's their implementation of `toString`. But there's a hidden feature that makes `toString` actually much more powerful than that. We can use it as an extended `typeof` and an alternative for `instanceof`.

Sounds strange? Indeed. Let's demystify.

By [specification](https://tc39.github.io/ecma262/#sec-object.prototype.tostring), the built-in `toString` can be extracted from the object and executed in the context of any other value. And its result depends on that value.

- For a number, it will be `[object Number]`
- For a boolean, it will be `[object Boolean]`
- For `null`: `[object Null]`
- For `undefined`: `[object Undefined]`
- For arrays: `[object Array]`
- ...etc (customizable).

Let's demonstrate:

```js run
// copy toString method into a variable for convenience
let objectToString = Object.prototype.toString;

// what type is this?
let arr = [];

alert( objectToString.call(arr) ); // [object *!*Array*/!*]
```

Here we used [call](mdn:js/function/call) as described in the chapter [](info:call-apply-decorators) to execute the function `objectToString` in the context `this=arr`.

Internally, the `toString` algorithm examines `this` and returns the corresponding result. More examples:

```js run
let s = Object.prototype.toString;

alert( s.call(123) ); // [object Number]
alert( s.call(null) ); // [object Null]
alert( s.call(alert) ); // [object Function]
```

### Symbol.toStringTag

The behavior of Object `toString` can be customized using a special object property `Symbol.toStringTag`.

For instance:

```js run
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]
```

For most environment-specific objects, there is such a property. Here are some browser specific examples:

```js run
// toStringTag for the environment-specific object and class:
alert( window[Symbol.toStringTag]); // Window
alert( XMLHttpRequest.prototype[Symbol.toStringTag] ); // XMLHttpRequest

alert( {}.toString.call(window) ); // [object Window]
alert( {}.toString.call(new XMLHttpRequest()) ); // [object XMLHttpRequest]
```

As you can see, the result is exactly `Symbol.toStringTag` (if exists), wrapped into `[object ...]`.

At the end we have "typeof on steroids" that not only works for primitive data types, but also for built-in objects and even can be customized.

We can use `{}.toString.call` instead of `instanceof` for built-in objects when we want to get the type as a string rather than just to check.

## Summary

Let's summarize the type-checking methods that we know:

|               | works for   |  returns      |
|---------------|-------------|---------------|
| `typeof`      | primitives  |  string       |
| `{}.toString` | primitives, built-in objects, objects with `Symbol.toStringTag`   |       string |
| `instanceof`  | objects     |  true/false   |

As we can see, `{}.toString` is technically a "more advanced" `typeof`.

And `instanceof` operator really shines when we are working with a class hierarchy and want to check for the class taking into account inheritance.
