# Բնիկ նախատիպեր

Այս `«prototype»` հատկությունը լայնորեն օգտագործվում է հենց JavaScript-ի միջուկի կողմից: Բոլոր ներկառուցված կոնստրուկտոր ֆունկցիաներն օգտագործում են այն:

First we'll see at the details, and then how to use it for adding new capabilities to built-in objects.
Սկզբում մենք կդիտարկենք մանրամասները, իսկ հետո՝ ինչպես օգտագործել այն ներկառուցված օբյեկտներին նոր հնարավորություններ ավելացնելու համար:

## Object.prototype

Ենթադրենք, մենք դատարկ օբյեկտ ենք արտատպում.

```js run
let obj = {};
alert( obj ); // "[object Object]" ?
```

Որտե՞ղ է `«[object Object]»` տողը ստեղծող կոդը: Սա ներկառուցված `toString` մեթոդ է, բայց որտե?ղ է այն, `obj`-ն դատարկ է․․․

...Սակայն `obj = {}` հակիրճ նշումը նույնն է, ինչ `obj = new Object()`, որտեղ `Object`-ը ներկառուցված օբյեկտի կոնստրուկտոր ֆունկցիա է՝ իր սեփական `prototype`-ով, որը հղում է անում հսկայական օբյեկտին՝ իր `toString` և այլ մեթոդներով:

Ահա թե ինչ է կատարվում.

![](object-prototype.svg)

Երբ `new Object()` է կանչվում (կամ բառացիորեն ստեղծվում է օբյեկտ՝ `{...}`, դրա `[[Prototype]]`-ը լինում է `Object.prototype`՝ համաձայն նախորդ գլխում մեր քննարկած կանոնի:

![](object-prototype-1.svg)

Այսպիսով, երբ `obj.toString()` է կանչվում, մեթոդը վերցվում է `Object.prototype`-ից:

Մենք կարող ենք ստուգել այն այսպես.

```js run
let obj = {};

alert(obj.__proto__ === Object.prototype); // true

alert(obj.toString === obj.__proto__.toString); //true
alert(obj.toString === Object.prototype.toString); //true
```

Նկատի ունեցեք, որ `Object.prototype`-ի վերին շղթայում այլևս չկա `[[Prototype]]`․

```js run
alert(Object.prototype.__proto__); // null
```

## Այլ ներկառուցված նախատիպեր

Other built-in objects such as `Array`, `Date`, `Function` and others also keep methods in prototypes.

For instance, when we create an array `[1, 2, 3]`, the default `new Array()` constructor is used internally. So `Array.prototype` becomes its prototype and provides methods. That's very memory-efficient.

By specification, all of the built-in prototypes have `Object.prototype` on the top. That's why some people say that "everything inherits from objects".

Here's the overall picture (for 3 built-ins to fit):

![](native-prototypes-classes.svg)

Let's check the prototypes manually:

```js run
let arr = [1, 2, 3];

// it inherits from Array.prototype?
alert( arr.__proto__ === Array.prototype ); // true

// then from Object.prototype?
alert( arr.__proto__.__proto__ === Object.prototype ); // true

// and null on the top.
alert( arr.__proto__.__proto__.__proto__ ); // null
```

Some methods in prototypes may overlap, for instance, `Array.prototype` has its own `toString` that lists comma-delimited elements:

```js run
let arr = [1, 2, 3]
alert(arr); // 1,2,3 <-- the result of Array.prototype.toString
```

As we've seen before, `Object.prototype` has `toString` as well, but `Array.prototype` is closer in the chain, so the array variant is used.


![](native-prototypes-array-tostring.svg)


In-browser tools like Chrome developer console also show inheritance (`console.dir` may need to be used for built-in objects):

![](console_dir_array.png)

Other built-in objects also work the same way. Even functions -- they are objects of a built-in `Function` constructor, and their methods (`call`/`apply` and others) are taken from `Function.prototype`. Functions have their own `toString` too.

```js run
function f() {}

alert(f.__proto__ == Function.prototype); // true
alert(f.__proto__.__proto__ == Object.prototype); // true, inherit from objects
```

## Primitives

The most intricate thing happens with strings, numbers and booleans.

As we remember, they are not objects. But if we try to access their properties, temporary wrapper objects are created using built-in constructors `String`, `Number` and `Boolean`. They provide the methods and disappear.

These objects are created invisibly to us and most engines optimize them out, but the specification describes it exactly this way. Methods of these objects also reside in prototypes, available as `String.prototype`, `Number.prototype` and `Boolean.prototype`.

```warn header="Values `null` and `undefined` have no object wrappers"
Special values `null` and `undefined` stand apart. They have no object wrappers, so methods and properties are not available for them. And there are no corresponding prototypes either.
```

## Changing native prototypes [#native-prototype-change]

Native prototypes can be modified. For instance, if we add a method to `String.prototype`,  it becomes available to all strings:

```js run
String.prototype.show = function() {
  alert(this);
};

"BOOM!".show(); // BOOM!
```

During the process of development, we may have ideas for new built-in methods we'd like to have, and we may be tempted to add them to native prototypes. But that is generally a bad idea.

```warn
Prototypes are global, so it's easy to get a conflict. If two libraries add a method `String.prototype.show`, then one of them will be overwriting the method of the other.

So, generally, modifying a native prototype is considered a bad idea.
```

**In modern programming, there is only one case where modifying native prototypes is approved. That's polyfilling.**

Polyfilling is a term for making a substitute for a method that exists in the JavaScript specification, but is not yet supported by a particular JavaScript engine.

We may then implement it manually and populate the built-in prototype with it.

For instance:

```js run
if (!String.prototype.repeat) { // if there's no such method
  // add it to the prototype

  String.prototype.repeat = function(n) {
    // repeat the string n times

    // actually, the code should be a little bit more complex than that
    // (the full algorithm is in the specification)
    // but even an imperfect polyfill is often considered good enough
    return new Array(n + 1).join(this);
  };
}

alert( "La".repeat(3) ); // LaLaLa
```


## Borrowing from prototypes

In the chapter <info:call-apply-decorators#method-borrowing> we talked about method borrowing.

That's when we take a method from one object and copy it into another.

Some methods of native prototypes are often borrowed.

For instance, if we're making an array-like object, we may want to copy some `Array` methods to it.

E.g.

```js run
let obj = {
  0: "Hello",
  1: "world!",
  length: 2,
};

*!*
obj.join = Array.prototype.join;
*/!*

alert( obj.join(',') ); // Hello,world!
```

It works because the internal algorithm of the built-in `join` method only cares about the correct indexes and the `length` property. It doesn't check if the object is indeed an array. Many built-in methods are like that.

Another possibility is to inherit by setting `obj.__proto__` to `Array.prototype`, so all `Array` methods are automatically available in `obj`.

But that's impossible if `obj` already inherits from another object. Remember, we only can inherit from one object at a time.

Borrowing methods is flexible, it allows to mix functionalities from different objects if needed.

## Summary

- All built-in objects follow the same pattern:
    - The methods are stored in the prototype (`Array.prototype`, `Object.prototype`, `Date.prototype`, etc.)
    - The object itself stores only the data (array items, object properties, the date)
- Primitives also store methods in prototypes of wrapper objects: `Number.prototype`, `String.prototype` and `Boolean.prototype`. Only `undefined` and `null` do not have wrapper objects
- Built-in prototypes can be modified or populated with new methods. But it's not recommended to change them. The only allowable case is probably when we add-in a new standard, but it's not yet supported by the JavaScript engine
