
# Նախատիպի մեթոդներ, օբյեկտներ առանց __proto__

Այս բաժնի առաջին գլխում մենք նշեցինք, որ կան նախատիպի կարգավորման ժամանակակից մեթոդներ:

`__proto__`-ն որոշ չափով համարվում է հնացած (սպասարկվում է JavaScript ստանդարտի միայն բրաուզերային հատվածում):

Ժամանակակից մեթոդներն են.

- [Object.create(proto, [descriptors])](mdn:js/Object/create) -- տրված `proto`-ով ստեղծում է դատարկ օբյեկտ՝ որպես `[[Prototype]]` և կամընտիր հատկությունների նկարագրիչներ:
- [Object.getPrototypeOf(obj)](mdn:js/Object/getPrototypeOf) -- վերադարձնում է `obj`-ի `[[Prototype]]`-ը։
- [Object.setPrototypeOf(obj, proto)](mdn:js/Object/setPrototypeOf) -- տեղադրում է `obj`-ի `[[Prototype]]`-ը որպես `proto`։

Սրանք պետք է օգտագործվեն `__proto__`-ի փոխարեն:

Օրինակ․

```js run
let animal = {
  eats: true
};

// ստեղծել նոր օբյեկտ, որի նախատիպը animal-ն է
*!*
let rabbit = Object.create(animal);
*/!*

alert(rabbit.eats); // true

*!*
alert(Object.getPrototypeOf(rabbit) === animal); // true
*/!*

*!*
Object.setPrototypeOf(rabbit, {}); // rabbit-ի նախատիպը փոփոխել {}-ի
*/!*
```

`Object.create`-ն ունի կամընտիր երկրորդ արգումենտ՝ հատկության նկարագրիչներ: Այնտեղ մենք կարող ենք լրացուցիչ հատկություններ տրամադրել նոր օբյեկտին հետևյալ եղանակով.

```js run
let animal = {
  eats: true
};

let rabbit = Object.create(animal, {
  jumps: {
    value: true
  }
});

alert(rabbit.jumps); // true
```

Նկարագրիչները նույն ձևաչափով են, ինչպես նկարագրված է <info:property-descriptors> գլխում:

We can use `Object.create` to perform an object cloning more powerful than copying properties in `for..in`:
Մենք կարող ենք օգտագործել `Object.create`՝ օբյեկտի կլոնավորումն ավելի հզոր ձևով իրականացնելու համար, քան `for..in`-ով հատկություններ պատճենելն է․

```js
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

Այս կանչը ստեղծում է `obj`-ի համար իսկապես ճշգրիտ պատճեն՝ ներառյալ բոլոր հատկությունները. թվարկելի և անթվարկելի հատկությունները, տվյալային հատկություններն ու սեթթերներ/գեթթերները՝ ամեն ինչ, ու ճշգրիտ `[[Prototype]]`-ով:

## Համառոտ պատմություն

Եթե հաշվենք `[[Prototype]]`-ը կառավարելու բոլոր եղանակները՝ դրանք շատ են: Նույն բանն անելու համար բազմաթիվ եղանակներ կան:

Ինչո՞ւ։

Պատմական պատճառներով է այդպես։

- Կոնստրուկտոր ֆունկցիայի `«prototype»` հատկությունը գործել է շատ հին ժամանակներից:
- Ավելի ուշ՝ 2012 թվականին, ստանդարտում հայտնվեց `Object.create`-ը: Այն նշված նախատիպով օբյեկտներ ստեղծելու հնարավորություն էր տալիս, սակայն ստանալու/տեղադրելու հնարավորություն չէր տալիս։ Այսպիսով, բրաուզերները ներդրեցին ոչ ստանդարտ `__proto__` աքսեսորը (accessor), որn օգտվողին թույլ էր տալիս ցանկացած պահի ստանալ/տեղադրել նախատիպ:
- Ավելի ուշ՝ 2015 թվականին, ստանդարտում ավելացվեցին `Object.setPrototypeOf`-ը և `Object.getPrototypeOf`-ը, որպեսզի կատարեն նույն գործառույթը, ինչ `__proto__`-ն է կատարում: Քանի որ `__proto__`-ը դե-ֆակտո ներդրվել էր ամենուր և այն մի տեսակ հնացած էր, իր ճանապարհը մտավ ստանդարտի B Հավելված, այսինքն՝ կամընտիր դարձավ ոչ բրաուզերային միջավայրերի համար:

Այս պահի դրությամբ մեր տրամադրության տակ են այս բոլոր տարբերակները։

Ինչո՞ւ `__proto__`-ն փոխարինվեց `getPrototypeOf/setPrototypeOf` ֆունկցիաներով: Սա հետաքրքիր հարց է, որը պահանջում է հասկանալ, թե ինչու է `__proto__`-ն վատ: Պատասխանը ստանալու համար շարունակեք ընթերցել։

```warn header="Don't change `[[Prototype]]` on existing objects if speed matters"
Technically, we can get/set `[[Prototype]]` at any time. But usually we only set it once at the object creation time and don't modify it anymore: `rabbit` inherits from `animal`, and that is not going to change.

And JavaScript engines are highly optimized for this. Changing a prototype "on-the-fly" with `Object.setPrototypeOf` or `obj.__proto__=` is a very slow operation as it breaks internal optimizations for object property access operations. So avoid it unless you know what you're doing, or JavaScript speed totally doesn't matter for you.
```

## "Very plain" objects [#very-plain]

As we know, objects can be used as associative arrays to store key/value pairs.

...But if we try to store *user-provided* keys in it (for instance, a user-entered dictionary), we can see an interesting glitch: all keys work fine except `"__proto__"`.

Check out the example:

```js run
let obj = {};

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // [object Object], not "some value"!
```

Here, if the user types in `__proto__`, the assignment is ignored!

That shouldn't surprise us. The `__proto__` property is special: it must be either an object or `null`. A string can not become a prototype.

But we didn't *intend* to implement such behavior, right? We want to store key/value pairs, and the key named `"__proto__"` was not properly saved. So that's a bug!

Here the consequences are not terrible. But in other cases we may be assigning object values, and then the prototype may indeed be changed. As a result, the execution will go wrong in totally unexpected ways.

What's worse -- usually developers do not think about such possibility at all. That makes such bugs hard to notice and even turn them into vulnerabilities, especially when JavaScript is used on server-side.

Unexpected things also may happen when assigning to `toString`, which is a function by default, and to other built-in methods.

How can we avoid this problem?

First, we can just switch to using `Map` for storage instead of plain objects, then everything's fine.

But `Object` can also serve us well here, because language creators gave thought to that problem long ago.

`__proto__` is not a property of an object, but an accessor property of `Object.prototype`:

![](object-prototype-2.svg)

So, if `obj.__proto__` is read or set, the corresponding getter/setter is called from its prototype, and it gets/sets `[[Prototype]]`.

As it was said in the beginning of this tutorial section: `__proto__` is a way to access `[[Prototype]]`, it is not `[[Prototype]]` itself.

Now, if we intend to use an object as an associative array and be free of such problems, we can do it with a little trick:

```js run
*!*
let obj = Object.create(null);
*/!*

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // "some value"
```

`Object.create(null)` creates an empty object without a prototype (`[[Prototype]]` is `null`):

![](object-prototype-null.svg)

So, there is no inherited getter/setter for `__proto__`. Now it is processed as a regular data property, so the example above works right.

We can call such objects "very plain" or "pure dictionary" objects, because they are even simpler than the regular plain object `{...}`.

A downside is that such objects lack any built-in object methods, e.g. `toString`:

```js run
*!*
let obj = Object.create(null);
*/!*

alert(obj); // Error (no toString)
```

...But that's usually fine for associative arrays.

Note that most object-related methods are `Object.something(...)`, like `Object.keys(obj)` -- they are not in the prototype, so they will keep working on such objects:


```js run
let chineseDictionary = Object.create(null);
chineseDictionary.hello = "你好";
chineseDictionary.bye = "再见";

alert(Object.keys(chineseDictionary)); // hello,bye
```

## Summary

Modern methods to set up and directly access the prototype are:

- [Object.create(proto, [descriptors])](mdn:js/Object/create) -- creates an empty object with a given `proto` as `[[Prototype]]` (can be `null`) and optional property descriptors.
- [Object.getPrototypeOf(obj)](mdn:js/Object/getPrototypeOf) -- returns the `[[Prototype]]` of `obj` (same as `__proto__` getter).
- [Object.setPrototypeOf(obj, proto)](mdn:js/Object/setPrototypeOf) -- sets the `[[Prototype]]` of `obj` to `proto` (same as `__proto__` setter).

The built-in `__proto__` getter/setter is unsafe if we'd want to put user-generated keys into an object. Just because a user may enter `"__proto__"` as the key, and there'll be an error, with hopefully light, but generally unpredictable consequences.

So we can either use `Object.create(null)` to create a "very plain" object without `__proto__`, or stick to `Map` objects for that.

Also, `Object.create` provides an easy way to shallow-copy an object with all descriptors:

```js
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

We also made it clear that `__proto__` is a getter/setter for `[[Prototype]]` and resides in `Object.prototype`, just like other methods.

We can create an object without a prototype by `Object.create(null)`. Such objects are used as "pure dictionaries", they have no issues with `"__proto__"` as the key.

Other methods:

- [Object.keys(obj)](mdn:js/Object/keys) / [Object.values(obj)](mdn:js/Object/values) / [Object.entries(obj)](mdn:js/Object/entries) -- returns an array of enumerable own string property names/values/key-value pairs.
- [Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols) -- returns an array of all own symbolic keys.
- [Object.getOwnPropertyNames(obj)](mdn:js/Object/getOwnPropertyNames) -- returns an array of all own string keys.
- [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys) -- returns an array of all own keys.
- [obj.hasOwnProperty(key)](mdn:js/Object/hasOwnProperty): returns `true` if `obj` has its own (not inherited) key named `key`.

All methods that return object properties (like `Object.keys` and others) -- return "own" properties. If we want inherited ones, we can use `for..in`.
