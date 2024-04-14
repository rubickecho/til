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

## 相关问题

- **React 中的事件处理与 DOM 中的事件处理有什么不同？**
    
    > React 中的事件处理使用的是合成事件（Synthetic Event）系统，而不是直接使用 DOM 事件。这意味着在 React 中，所有的事件处理都是通过一个中心化的事件处理器来进行的，这种方式称为事件委托。此外，React 的事件处理函数需要传入一个合成事件对象，而不是原生的 DOM 事件对象。
    
- **什么是合成事件（Synthetic Event）？它在 React 中的作用是什么？**
    
    > 合成事件是 React 中的一个概念，它是对原生 DOM 事件的封装。合成事件提供了与原生 DOM 事件相同的接口，但它具有跨浏览器的一致性。在 React 中，当一个事件发生时，会创建一个合成事件对象，并将其传递给事件处理函数。
    
- **React 中如何阻止事件的默认行为？为什么在事件处理函数中返回 `false` 不会阻止事件的默认行为？**
    
    > 在 React 中，你可以通过调用 `event.preventDefault()` 来阻止事件的默认行为。这与在原生 JavaScript 中阻止事件的默认行为的方式是一样的。然而，与原生 JavaScript 不同，你不能通过在事件处理函数中返回 `false` 来阻止事件的默认行为。这是因为在 React 中，事件处理函数的返回值并不会影响事件的默认行为。
    
- **什么是事件委托（Event Delegation）？React 是如何利用事件委托来提高性能的？**
    
    > 事件委托是一种事件处理模式，它是通过在一个父元素上监听事件，然后在事件发生时，根据事件的目标元素来找到并调用相应的事件处理函数。React 利用事件委托，将所有的事件处理器都绑定在文档的根节点上，而不是直接绑定在目标元素上。这样可以减少事件处理器的数量，从而提高性能。
    
- **在 React 中，如何在捕获阶段而不是冒泡阶段处理事件？**
    
    > 在 React 中，你可以通过添加 `Capture` 到事件处理器的名字来在捕获阶段处理事件。例如，你可以使用 `onClickCapture` 而不是 `onClick` 来在捕获阶段处理点击事件。当你这样做时，你的事件处理函数会在事件冒泡到目标元素之前被调用。