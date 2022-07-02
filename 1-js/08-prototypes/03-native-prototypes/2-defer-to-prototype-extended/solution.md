

```js run
Function.prototype.defer = function(ms) {
  let f = this;
  return function(...args) {
    setTimeout(() => f.apply(this, args), ms);
  }
};

// ստուգենք այն
function f(a, b) {
  alert( a + b );
}

f.defer(1000)(1, 2); // 1 վրկ հետո ցուցադրում է 3
```

Նկատի ունեցեք. մենք `this` ենք օգտագործում `f.apply`-ում, որպեսզի մեր դեկորացիան աշխատի օբյեկտների մեթոդների համար:

Այսպիսով, եթե պատյան ֆունկցիան կանչվում է որպես օբյեկտի մեթոդ, ապա `this`-ը փոխանցվում է սկզբնական `f` մեթոդին:

```js run
Function.prototype.defer = function(ms) {
  let f = this;
  return function(...args) {
    setTimeout(() => f.apply(this, args), ms);
  }
};

let user = {
  name: "John",
  sayHi() {
    alert(this.name);
  }
}

user.sayHi = user.sayHi.defer(1000);

user.sayHi();
```
