# React Refs

## 官方描述

> When you want a component to “remember” some information, but you don’t want that information to [trigger new renders](https://react.dev/learn/render-and-commit), you can use a *ref*.
> 

当你想让一个组件“记住”一些信息，但又不希望这些信息触发新的渲染时，你可以使用 ref。

## Why ref is “escape hatch”

在 React 中，"escape hatch" 是指一种可以绕过 React 的常规数据流和更新机制，直接操作 DOM 或组件实例的方式。Ref 就是这样一种 "escape hatch"。

在大多数情况下，应该避免使用 refs，因为它们使代码更难理解和维护。React 的声明式编程模式（即只需要描述 UI 应该是什么样子，React 就会自动更新 DOM）通常应该足够处理大多数 UI 更新的情况。

## What is callback ref?

## 为什么不能在 render 阶段使用?

官方文档这一段描述的很清楚:

> link: https://react.dev/learn/manipulating-the-dom-with-refs
> 
> 
> In React, every update is split in [two phases](https://react.dev/learn/render-and-commit#step-3-react-commits-changes-to-the-dom):
> 
> - During **render,** React calls your components to figure out what should be on the screen.
> - During **commit,** React applies changes to the DOM.
> 
> In general, you [don’t want](https://react.dev/learn/referencing-values-with-refs#best-practices-for-refs) to access refs during rendering. That goes for refs holding DOM nodes as well. During the first render, the DOM nodes have not yet been created, so `ref.current` will be `null`. And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them.
> 
> React sets `ref.current` during the commit. Before updating the DOM, React sets the affected `ref.current` values to `null`. After updating the DOM, React immediately sets them to the corresponding DOM nodes.
> 
> **Usually, you will access refs from event handlers.** If you want to do something with a ref, but there is no particular event to do it in, you might need an Effect. We will discuss Effects on the next pages.
> 

# Refs VS State

| refs | state |
| --- | --- |
| useRef(initialValue) returns { current: initialValue } | useState(initialValue) returns the current value of a state variable and a state setter function ( [value, setValue]) |
| Doesn’t trigger re-render when you change it. | Triggers re-render when you change it. |
| Mutable—you can modify and update current’s value outside of the rendering process. | ”Immutable”—you must use the state setting function to modify state variables to queue a re-render. |
| You shouldn’t read (or write) the current value during rendering. | You can read state at any time. However, each render has its own https://react.dev/learn/state-as-a-snapshot of state which does not change |

## 使用场景？

- 操作 DOM，由 React 提供的一些浏览器 API，会挂载到 ref.current 上.
    - 当 ref 附加到 DOM 元素（例如，`<div>`、`<input>`等）时，`ref.current` 将指向该 DOM 元素。你可以通过 `ref.current` 访问到所有的原生 DOM API。
- 跟踪组件中的状态，但又不触发组件重新渲染
    - 例如，配合 useEffect, we can track whether a component has been rendered for the first time or whether it has been re-rendered，参考 react-use [useFirstMountState](https://github.com/streamich/react-use/blob/master/docs/useUpdateEffect.md).
        
        ```jsx
        import { useRef } from 'react';
        
        export function useFirstMountState(): boolean {
          const isFirst = useRef(true);
        
          if (isFirst.current) {
            isFirst.current = false;
        
            return true;
          }
        
          return isFirst.current;
        }
        ```
        
- 访问子组件实例（数据或者调用子组件暴露的方法）
    - 当 ref 附加到类组件时，`ref.current` 将指向该组件的实例，你可以通过 `ref.current` 访问到组件的所有公共方法和属性。需要注意的是，函数组件没有实例，因此不能使用 ref，除非它们使用 `React.forwardRef`。
    

## 参考问题

- **什么是 React ref？请解释其用途。**
    
    React ref 是一种可以让我们直接访问 DOM 节点或在 render 方法中创建的 React 元素的方式。它们通常用于读取 DOM 属性（如输入框的值），或者触发对焦、动画等需要直接操作 DOM 的行为。
    
- **在类组件和函数组件中如何创建和使用 ref？**
    
    在类组件中，通常在构造函数中使用 `React.createRef()` 创建 ref，并在需要的地方通过 `this.myRef.current` 访问。
    
    ```jsx
    class MyComponent extends React.Component {
      constructor(props) {
        super(props);
        this.myRef = React.createRef();
      }
    
      render() {
        return <div ref={this.myRef} />;
      }
    }
    
    ```
    
    在函数组件中，可以使用 `React.useRef()` Hook 创建 ref。
    
    ```jsx
    function MyComponent() {
      const myRef = React.useRef();
    
      return <div ref={myRef} />;
    }
    
    ```
    
- **什么是 `React.forwardRef`？请给出一个使用它的场景。**
    
    `React.forwardRef` 是一种特殊的 React API，它可以让你将 ref 从父组件传递到子组件。这在你需要在父组件中访问子组件的 DOM 节点时非常有用。
    
    ```jsx
    const MyComponent = React.forwardRef((props, ref) => (
      <div ref={ref} {...props} />
    ));
    
    function ParentComponent() {
      const myRef = React.useRef();
    
      return <MyComponent ref={myRef} />;
    }
    
    ```
    
- **解释一下什么是 "callback refs"，并与 `React.createRef()` 的区别。**
    
    "callback refs" 是一种创建 ref 的方式，它接受一个函数，当 ref 被附加到元素时，这个函数会被调用，并接收该元素作为参数。与 `React.createRef()` 不同，"callback refs" 提供了更多的灵活性，例如，你可以在 ref 被设置或取消时执行一些额外的操作。
    
    ```jsx
    class MyComponent extends React.Component {
      setMyRef = (element) => {
        this.myRef = element;
      }
    
      render() {
        return <div ref={this.setMyRef} />;
      }
    }
    
    ```
    
- **在什么情况下我们应该使用 ref？在什么情况下我们应该避免使用 ref？**
    
    当你需要直接操作 DOM，例如，管理焦点、文本选择或媒体播放，触发强制动画，集成第三方 DOM 库时，你应该使用 ref。
    
    然而，在大多数情况下，你应该避免使用 ref，因为它们使得你的代码更难理解和维护。React 的声明式编程模式（即你只需要描述 UI 应该是什么样子，React 就会自动更新 DOM）通常应该足够处理大多数 UI 更新的情况。
    
- 如果是在 function component 中实现 callback ref，不用 useCallback 包裹，会导致每次组件渲染时都触发？
    
    在函数组件中，如果直接将一个函数作为 ref（即 callback ref），那么每次组件渲染时，这个函数都会被调用两次——一次是用 `null` 参数（因为旧的 ref 被清除），一次是用 DOM 元素参数（因为新的 ref 被设置）。
    
    为了避免这种情况，可以使用 `React.useCallback` Hook 来包装 callback ref。`useCallback` 会返回一个记忆化的版本的 callback，它只在依赖项改变时才会更新。这样，只有当组件挂载或卸载，或者依赖项改变时，callback ref 才会被调用。
    
    以下是一个使用 `useCallback` 的例子：
    
    ```jsx
    import React, { useCallback } from 'react';
    
    function MyComponent() {
      const setMyRef = useCallback((node) => {
        if (node) {
          node.focus();
        }
      }, []);
    
      return <input type="text" ref={setMyRef} />;
    }
    
    ```
    
    在这个例子中，`setMyRef` 是一个 callback ref，它被 `useCallback` 包装，所以它只在组件挂载或卸载时被调用，而不是每次组件渲染时。