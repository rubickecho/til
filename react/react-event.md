# React Event

## React Event VS DOM Event

- 事件名称
    - 在 React 中，事件的名称是驼峰命名法，在 DOM 中是小写
    - 例如：`onclick ⇒ onClick`
- 事件处理函数
    - 在 React 中，事件处理器是一个函数，在 DOM 中，事件处理器是字符串
    - 例如：`onClick={handleClick()}`、`onclick=”handleClick()”`
- 事件对象
    - 在 React 中，事件对象是 React 的合成事件，而不是浏览器的原生事件（为了确保所有浏览器的一致性）。虽然和原生事件对象的 API 非常相似，但是还是有一些差异
    - 例如：保持事件对象的引用 `event.perisist()`
- 默认行为的阻止
    - 在 React 中，需要调用 `event.preeventDefault(`) 来阻止事件的默认行为，在 JS 中，也是用这个方法来阻止，但是也可以 `return false` 来阻止默认行为。

## React 事件绑定原理

react 实现了一套自己的事件处理系统：合成事件（Synthetic Event），是为了为所有的浏览器提供一致的处理方式。

以下是 React 事件处理主要步骤：

1. **事件委托：** React 不会直接在 DOM 元素上绑定事件处理器，而是在文档的根节点上监听所有的事件。当一个事件发生时，React 会根据事件的类型和目标元素，找到对应的事件处理器并调用它。这种方式称为事件委托，它可以减少事件监听器的数量，从而提高性能。
2. **合成事件：** 当一个事件发生时，React 会创建一个 `SyntheticEvent` 对象。这个对象包含了所有的事件信息，并且它的 API 与原生的 DOM 事件对象的 API 是一致的。这样，无论你在哪个浏览器中使用 React，你都可以以相同的方式处理事件。
3. **事件池：** 为了提高性能，React 会重用 `SyntheticEvent` 对象。当一个事件处理器被调用后，React 会清空 `SyntheticEvent` 对象的所有属性，并将它放回事件池。这意味着，你不能在异步代码中使用 `SyntheticEvent` 对象，因为它可能已经被重用。
4. **事件处理函数：** 在 React 中，你需要将一个函数作为事件处理器，而不是一个字符串。这个函数会接收一个 `SyntheticEvent` 对象作为参数。你可以在这个函数中调用 `event.preventDefault()` 或 `event.stopPropagation()` 来阻止事件的默认行为或传播。

### 源代码位置

`packages/react-dom/src/events/SyntheticEvent.js`

具体可以见：[React event object](https://react.dev/reference/react-dom/components/common#react-event-object)

## What is `onClickCapture`?

> Each event propagates in three phases:
> 
> 1. It travels down, calling all `onClickCapture` handlers.
> 2. It runs the clicked element’s `onClick` handler.
> 3. It travels upwards, calling all `onClick` handlers.

`onClickCapture` 是 React 中的一个特殊事件处理器，它在事件捕获阶段而不是冒泡阶段处理点击事件。

在 DOM 事件中，当一个事件发生时，它会经过两个阶段：捕获阶段和冒泡阶段。在捕获阶段，事件从顶层元素（如 `document` 或 `window`）开始，向下传递到目标元素。然后，在冒泡阶段，事件从目标元素开始，向上回到顶层元素。

通常，我们在 React 中使用的事件处理器（如 `onClick`）是在冒泡阶段处理事件的。但有时，你可能希望在捕获阶段处理事件。这就是 `onClickCapture` 的用途。