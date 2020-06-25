# 装饰器（decorator）

test
设计一个装饰器来方便的指定模块所需要的依赖

```js
function Module(metadata) {
  return function (constructor) {
    return constructor;
  };
}
```

如果我们需要定制装饰器，这个时候就需要一个工厂函数，返回一个装饰器

装饰器类别：

1. 类装饰器，声明在类之前

   类装饰器的参数是类本身

2. 方法，属性，访问器（get）装饰器

   这种装饰器声明时，会被当作函数调用，并接收三个参数：target,key,prototype descriptor;
   属性装饰器的第三个参数是 `undefined`;
   静态属性与实例属性的第一个参数 target 不同，其中静态属性是类的构造函数，实例属性是类的原型对象 prototype

```ts
function decorator(target: any, key: string, descriptor: PropertyDescriptor) {}

class Demo {
  // target -> Demo.prototype
  // key -> 'demo1'
  // descriptor -> undefined
  @decorator
  demo1: string;

  // target -> Demo
  // key -> 'demo2'
  // descriptor -> PropertyDescriptor类型
  @decorator
  static demo2: string = "demo2";

  // target -> Demo.prototype
  // key -> 'demo3'
  // descriptor -> PropertyDescriptor类型
  @decorator
  get demo3() {
    return "demo3";
  }

  // target -> Demo.prototype
  // key -> 'method'
  // descriptor -> PropertyDescriptor类型
  method() {}
}
```
