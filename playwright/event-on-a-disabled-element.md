
# Event on a disabled element

## 问题描述

在 playwright 时，想要检查一个 disabled 的 Button 是否正常显示 tooltip,
具体 [代码示例](https://codepen.io/peakcool/pen/RwYPZjB?editors=0010), 但是一直没有生效。
测试代码如下

```js
await page.locator('button').hover();
await page.waitForSelector('div[role="tooltip"]', { hasText: 'i am message' });
```

## 问题分析

主要参考 stackoverflow 上 [Event on a disabled input](https://stackoverflow.com/questions/3100319/event-on-a-disabled-input)

> Disabled elements don't fire mouse events. Most browsers will propagate an event originating from the disabled element up the DOM tree, so event handlers could be placed on container elements. However, Firefox doesn't exhibit this behaviour, it just does nothing at all when you click on a disabled element.
> 被禁用的元素不会触发鼠标事件。大多数浏览器会将来自禁用元素的事件传播到DOM树上，因此事件处理程序可以放在容器元素上。然而，Firefox并没有表现出这种行为，当你点击一个被禁用的元素时，它根本就不做任何事情。

所以，这也可以解释为什么在 playwright 中，hover 一个 disabled 的 button 时，没有触发 tooltip 的显示。

## 解决方案

将 hover 的元素改为 disabled 的父元素，就可以正常显示 tooltip。

```js
await page.locator('button').parent().hover();
await page.waitForSelector('div[role="tooltip"]', { hasText: 'i am message' });
```

## 参考
  * [Event on a disabled input](https://stackoverflow.com/questions/3100319/event-on-a-disabled-input)
  * [UI Events](https://w3c.github.io/uievents/#event-type-click)
  * [4.10.18.5 Enabling and disabling form controls: the disabled attribute](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#attr-fe-disabled)
  * [Hover not working on disabled input field?](https://stackoverflow.com/questions/18941700/hover-not-working-on-disabled-input-field)