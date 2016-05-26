推荐阮一峰老师的[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)

下面是实际项目中写 ES6 代码时的注意点：

1、 `Object.assign()` 中解决 `target` 为 `null` 的情况：
```
let target = null;
const source = { a: 1 };
target = Object.assign({}, target, source);
```
