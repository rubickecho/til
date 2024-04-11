# What is React Render Props？

什么是 React 渲染属性？它是一种组件实现模式，和高阶组件类似（HOC）

一个组件通过 props 接收一个函数，并且这个组件不仅返回 React 元素，还调用接收的函数，然后将某些状态或行为作为参数传递给这个函数。

> A render prop is a prop on a component, which value is a function that returns a JSX element. The component itself does not render anything besides the render prop. Instead, the component simply calls the render prop, instead of implementing its own rendering logic.
> 

render prop 是组件上的一个 prop，其值是一个返回 JSX 元素的函数。组件本身除了 render prop 之外不会渲染任何内容。相反，组件只是调用 render prop，而不实现自己的渲染逻辑。

这个组件 prop，可以是显式定义的属性，也可以是默认的 children，例如

```jsx
class DataProvider extends React.Component {
  state = { data: "Hello" };

  render() {
    // 使用 `this.props.render` 函数，并将 `data` 作为参数传递
    return this.props.render(this.state.data);
  }
}

// 使用 DataProvider 组件
<DataProvider render={data => <h1>{data}</h1>} />
```

```jsx
class DataProvider extends React.Component {
  state = { data: "Hello" };

  render() {
    // 使用 `this.props.render` 函数，并将 `data` 作为参数传递
    return this.props.children(this.state.data);
  }
}

// 使用 DataProvider 组件
<DataProvider>
	{(data)=> <h1>{data}</h1>}
</DataProvider>
```

# Pros and Cons

优点：

- 提高组件的可重用性，这点和 HOC 一样，可以共享逻辑和数据给多个组件。
- 相比 HOC，不会自动合并属性，所以也不会遇到命名冲突问题，所以属性都显式的传递给子组件。
- 逻辑与渲染组件分离，接收 render props 的有状态组件可以将数据传递给无状态组件，后者可以更加关注视图实现

缺点：

- 大部分情况下，可以用 HOOKS 代替实现
- “Since we can’t add lifecycle methods to a **`render`** prop, we can only use it on components that don’t need to alter the data they receive.”