# 怎么设计一套 ioc

一套 ioc 应该至少具备 [injector](https://github.com/JackZong/Learning-notes/blob/master/Part1/Practice/IOC/injector.md)，provider，container 三个模块

# provider

描述了一个 injector 应该怎样创建一个所依赖的对象实例。每个 provider 都至少应该要有一个唯一的 token 和对应的对象，其中对象可以是 object，function，class 或者 js 的基本类型数据，所以我们可以衍生出 class provider，value provider 等。

根据 provider 的特性，我们来创建一个基本类 Provider

```ts
class Provider {
  constructor(token, instance) {
    this.token = token;
    this.instance = instance;
  }

  setInstance(instance) {
    return this.instance;
  }

  getInstance() {
    return this.instance;
  }

  resolved() {
    return this.instance !== null;
  }
}
```

然后我们再来创建一个 value provider

```ts
class ValueProvider extends Provider {
  constructor(token, instance) {
    super(token, instance);
    //value provider的instance会直接是一个对象
    this.setInstance({
      value: this.value,
    });
  }
}
```

创建一个 class provider

```ts
class ClassProvider extends provider {
  constructor(token, klass, deps, isPrivate) {
    super(token, instance);
    this.klass = klass;
    tihs.deps = deps;
  }
}
```
