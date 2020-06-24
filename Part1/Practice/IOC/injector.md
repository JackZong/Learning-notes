# Injector 模块

Injector 实现了依赖注入，根据不同 provider 的类型，实例化 provider，然后将 provider instance 传给需要的 module

# Injector 有很多种实现方式

## 1. 基于 Injector、Cache 和函数参数名的依赖注入

```js
class Injector {
  //暂存模块中方法所需的依赖对象
  _cache = {};

  getParamsName(func) {
    //通过regex 和 toString() 获取模块所需的依赖参数
    return "paramsName";
  }

  put(depName, depInstance) {
    if (!this._cache.depName) {
      this._cache[depName] = depInstance;
      console.log(`add ${depName} successful!`);
    }
  }

  resolve(func, bind) {
    var self = this;
    const paramsName = bind.deps;
    const params = paramsName.map((paramName) => self._cache[paramName]);
    func.apply(bind, params);
  }

  resolveClass(klass, funcName) {
    const self = this;
    if (typeof klass !== "function") return;
    // 因为现代前端工具在build代码时可能会将代码uglify，所以不能简单的通过参数名称来获取模块对应的依赖，这时
    // 需要主动式的声明某个模块对应的依赖
    const paramsName = klass.prototype.deps || getParamsName(klass);
    const params = paramsName.map((paramName) => ({
      [paramName]: self._cache[paramName],
    }));
    const paramObj = {};
    params.forEach((obj) => {
      paramObj = Object.assign(paramObj, obj);
    });
    // 这里作为测试直接调用，实际使用会将instance存到container中
    const classInstance = new klass(paramObj);
    classInstance[funcName]();
  }
}
```

```js
class Test {
  constructor({ a }) {
    this._a = a;
  }
  echo() {
    if (this._a) {
      console.log("get a");
    } else {
      console.log("cant get a");
    }
  }
}
function a() {}
// 可以封装成一个decorator，如@Module();
Test.prototype.deps = ["a"];

const injector = new Injector();

injector.put("a", a);
injector.resolveClass(Test, "echo");
```

**codepen**

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="js,result" data-user="jackzong" data-slug-hash="oNbwjLW" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Simple DI">
  <span>See the Pen <a href="https://codepen.io/jackzong/pen/oNbwjLW">
  Simple DI</a> by JinZhang (<a href="https://codepen.io/jackzong">@jackzong</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
