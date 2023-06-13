# 如何判断一个方法是原生方法

使用 Function.prototype.toString 方法：另一种方法是使用函数对象的 toString 方法，并检查其返回值中是否包含 `native code`。

```js
/**
 * 判断是否为原生方法
 * @param {Function} fn 函数
 * @returns {boolean}
 */
function isNative(fn) {
  // '' + fn 利用了js的隐式类型转换，也可以 fn.toString()
  return (/\{\s*\[native code\]\s*\}/).test('' + fn);
}
```

## Example

```js
isNative(alert); // true
isNative(isNative); // false
```