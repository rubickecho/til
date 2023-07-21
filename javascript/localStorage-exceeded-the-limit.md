# How to judge that the browser's localStorage storage has reached the upper limit

根据 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage/setItem) 的描述，localStorage 存储的上限是 5M，但是实际上不同浏览器的上限是不一样的，比如 Chrome 是 10M，Firefox 是 5M，Safari 是 5M，IE 是 10M。

在绝大多数情况下，可以根据 localStorage 的返回值来判断是否达到上限，但是在某些情况下，比如 Safari 浏览器，即使达到上限，也不会返回错误，因此需要使用 try catch 来捕获异常。

```js
 /**
 * set localstorage
 * @param {string} key key
 * @param {*} value 设置值
 */
function localStorageSetItem(key, value) {
  // 使用 localStorage 会有容量上限，因此需要进行 try catch 处理
  try {
    localStorage.setItem(key, value);
  } catch (e) {
    if (['QuotaExceededError', 'NS_ERROR_DOM_QUOTA_REACHED'].includes(e.name)) {
      console.warn('localStorage 存储已达上限！')
    }
  }
}
```