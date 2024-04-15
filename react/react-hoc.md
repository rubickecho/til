# What is React HOC

HOC（高阶组件） 是一个函数，接收一个组件并返回一个新的组件。

我理解为：跳板组件 or 增强组件

## React HOC Snippet

### Return Class Component

```jsx
function withSubscription(WrappedComponent, selectData) {
  // ...并返回另一个组件...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ...负责订阅相关的操作...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      // ...并取消订阅...
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... 并使用新数据渲染被包装的组件!
      // 请注意，我们可能还会传递其他属性
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

### Return Function Component

```jsx
function withSubscription(WrappedComponent, selectData) {
  // 返回一个函数组件
  return function WithSubscription(props) {
    const [data, setData] = React.useState(selectData(DataSource, props));

    React.useEffect(() => {
      function handleChange() {
        setData(selectData(DataSource, props));
      }

      // 订阅数据
      DataSource.addChangeListener(handleChange);

      // 取消订阅
      return () => {
        DataSource.removeChangeListener(handleChange);
      };
    }, [props]);

    // 使用新数据渲染被包装的组件
    return <WrappedComponent data={data} {...props} />;
  };
}
```

## 应用场景

- 数据获取和订阅
- 条件渲染，比如加载数据时，`withLoading`
- 错误处理：统一捕获和处理错误，比如 `withErrorBoundary`
- 样式主题：将主题样式传递给被包装的组件，比如 `withTheme`
- Redux 链接，比如 `Redux connect`
- 路由信息，比如 `React Router withRouter`

## React VS Vue HOC

相同点：

1. **抽象和重用逻辑**：无论在 React 还是 Vue 中，HOC 都是用于抽象和重用组件逻辑的一种模式。通过将逻辑封装在 HOC 中，可以在多个组件之间重用这些逻辑，而无需重复编写代码。
2. **接收组件并返回新组件**：无论在 React 还是 Vue 中，HOC 都是一个函数，它接收一个组件作为参数，并返回一个新的组件。返回的组件会添加或修改一些逻辑，然后渲染原始的组件。

不同点：

1. **语法和实现方式**：由于 React 和 Vue 的设计和语法差异，它们的 HOC 实现方式也有所不同。在 React 中，HOC 是一个返回组件的函数，而在 Vue 中，HOC 通常是一个返回组件选项对象的函数。
2. **作用域插槽 vs. props**：在 Vue 的 HOC 中，通常会使用作用域插槽来传递数据和方法给被包装的组件。而在 React 的 HOC 中，通常会使用 props 来传递数据和方法给被包装的组件。
3. **`React Hook` vs. `Vue Composition API`**：React 和 Vue 都引入了新的 API（React Hook 和 Vue Composition API）来更好地支持逻辑重用和代码组织。这些新的 API 提供了一种替代 HOC 的方式来重用和组织代码。

在 Vue 中，我常用做跳板组件，传入组件对象，渲染该组件，比如做低代码组件渲染对象时，抽离基础渲染组件，根据低代码组件配置渲染对应组件和其逻辑。

```jsx
/**
 * 高阶组件，传递参数生成另一个组件
 */
function hoc(h, options) {
	const { name, on, attrs, props, scopedSlots } = options;
	return h(name, {
		on: on, // 监听方法
		attrs: attrs, // 未被声明的属性
		props: props, // 高阶组件接收到的props属性
		scopedSlots: scopedSlots // 插槽
	});
}

export default hoc;
```

## React Hook VS React HOC

> First, usually a React Hook does not return conditional JSX. And secondly, a React Hook is not guarding a component from the outside but rather adds implementation details in the inside.
> 

首先，通常情况下，React Hook 不会返回条件性的 JSX。其次，React Hook 并不是从外部保护组件的机制，而是在内部添加实现细节。

## 相关问题

- **什么是高阶组件（HOC）？请给出一个例子。**
    
    高阶组件是一个函数，它接受一个组件作为参数，并返回一个新的组件。例如，一个常见的 HOC 是 Redux 的 `connect` 函数，它将 Redux store 中的状态和 dispatch 函数映射到被包装的组件的 props。
    
- **高阶组件（HOC）有什么用途？**
    
    高阶组件可以用于处理各种逻辑，例如数据获取和订阅、条件渲染、错误处理、样式主题、Redux 连接、路由信息等。它们可以用于抽象和重用组件逻辑。
    
- **高阶组件（HOC）和 React Hook 有什么区别？**
    
    高阶组件是在组件外部使用的，它可以包装和保护组件。相比之下，React Hook 是在组件内部使用的，它可以让你在组件内部添加和管理状态、副作用等。虽然它们都可以用于重用组件逻辑，但它们的使用方式和目的是不同的。
    
- **在使用高阶组件（HOC）时，有哪些需要注意的问题？**
    
    在使用 HOC 时，需要注意一些问题，例如避免 prop 冲突（HOC 不应该使用可能与被包装的组件的 props 冲突的 prop 名称）、传递未知的 props（HOC 应该将接收到的任何未知的 props 都传递给被包装的组件）、复制静态方法（如果 HOC 返回的组件没有复制被包装的组件的静态方法，那么这些方法将丢失）等。
    
- **请解释一下 React 中的 `compose` 函数，以及它与高阶组件（HOC）的关系。**
    
    `compose` 函数是一个函数式编程的工具，它可以将多个函数组合成一个函数。在 React 中，`compose` 函数常常用于组合多个 HOC。例如，如果你有多个 HOC，你可以使用 `compose` 函数来组合它们，而不是嵌套调用它们。