# Mixin

在一个对象中混入另外一个对象的方法。

**栗子 🌰：**

```js
const Foo = {
  foo() {
    console.log("foo");
  },
};

class MyClass {}

Object.assign(MyClass.prototype, Foo);

const ins = new MyClass();

ins.foo(); // 'foo'
```

**栗子 🌰：用 Decorator 包装**

首先，Decorator 是 ES7 中的一个[提案]('https://github.com/wycats/javascript-decorators#class-declaration'),这个概念是从 Python 借鉴过来的，在 Python 里，decorator 实际上是一个 wrapper，它作用于一个目标函数，对这个目标函数做一些额外的操作，然后返回一个新的函数，ES7 中的 Decorator 也类似。

```js
function mixins(...params) {
  return function (target) {
    Object.assign(target.prototype, ...params);
  };
}

const Foo = {
  foo() {
    document.writeln("foo");
  },
};

@minxins
class MyClass {}

let obj = new MyClass();
obj.foo(); // 'foo', 会修改掉 MyClass 类的 prototype 对象
```

**栗子 🌰：用类的继承实现**

```js
const MyMixin = (superclass) =>
  class extends superClass {
    foo() {
      document.writeln("foo");
    }
  };

class MyBaseClass {}
class MyClass extends MyMixin(MyBaseClass) {}

const ins = new MyClass();
ins.foo(); // 'foo', 不会改掉 MyClass 类的 prototype 对象
```
