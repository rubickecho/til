# What is MutationObserver API

MutationObserver provides developers a way to react to changes in a DOM. It is designed as a replacement for Mutation Events defined in the DOM3 Events specification.

## Basic Usage

```js
let observerOptions = {
  childList: true,
  attributes: true,
  characterData: false,
  subtree: false,
  attributeFilter: ['one', 'two'],
  attributeOldValue: false,
  characterDataOldValue: false
};

const targetNode = document.getElementById('myId');
let observer = new MutationObserver(callback);
    
function callback (mutations) {
  // do something here
}

observer.observe(targetNode, observerOptions);
```

## Online Demo

- [MutationObserver](https://codesandbox.io/s/elegant-mcclintock-qkc5xy?file=/index.html)

## Reference

- [MDN](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)
- [Getting To Know The MutationObserver API](https://www.smashingmagazine.com/2019/04/mutationobserver-api-guide/)