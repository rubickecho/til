# 【翻译】React Hooks 迁移指南

- Last Edited: February 28, 2024 6:41 PM
- Tags: 翻译
- URL: https://www.robinwieruch.de/react-hooks-migration/

# React Hooks 迁移指南

React Hooks 的引入，让 React 函数组件也能够方便地使用状态和副作用，这一功能以前仅限于 React 类组件。随着 [React 组件实现方式的演变](https://www.robinwieruch.de/react-component-types/)，现在我们可以在函数组件中享受到类似类组件的特性了。

本教程将指导您如何将 React 类组件迁移为使用 [React Hooks](https://www.robinwieruch.de/react-hooks/) 的函数组件，包括状态管理和副作用的转换示例。

本教程的目的不是促使开发者将所有类组件都转换为函数组件，而是展示如何以函数组件实现类似的功能。这样，您可以根据需要，决定未来组件的实现方式。

## 使用 React 的 useState 钩子管理组件状态

在引入 Hooks 之前，实现有状态组件的标准做法是使用 React 类组件。可以在构造函数中初始化状态，在类方法中通过 `this.setState()` 方法更新状态，并通过 `this.state` 访问状态。

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: "" };
  }
  onChange = (event) => {
    this.setState({ value: event.target.value });
  };
  render() {
    return (
      <div>
        {" "}
        <h1>Hello React ES6 Class Component!</h1>{" "}
        <input value={this.state.value} type="text" onChange={this.onChange} />{" "}
        <p>{this.state.value}</p>{" "}
      </div>
    );
  }
}
```

现在，函数组件通过使用 `useState` 钩子也能做到同样的事情。这个钩子允许我们初始化状态（比如一个空字符串），并返回一个包含状态和更新状态函数的数组。通过 JavaScript 的数组解构，我们可以方便地在一行代码中提取这些值：

```jsx
const App = () => {
  const [value, setValue] = React.useState("");

  const onChange = (event) => setValue(event.target.value);
  return (
    <div>
      {" "}
      <h1>Hello React Function Component!</h1>{" "}
      <input value={value} type="text" onChange={onChange} /> <p>{value}</p>{" "}
    </div>
  );
};
```

本质上，函数组件因为不需要处理构造函数或类方法而更加轻量。考虑到状态管理，下次实现 React 组件时，可以考虑使用带有 React Hooks 的函数组件。

## 使用 React 的 useEffect 钩子处理副作用

接下来，让我们看看如何将类组件的副作用迁移到函数组件。这里，我们将通过引入浏览器本地存储作为副作用来进行展示：

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: localStorage.getItem("myValueInLocalStorage") || "" };
  }
  componentDidUpdate() {
    localStorage.setItem("myValueInLocalStorage", this.state.value);
  }
  onChange = (event) => {
    this.setState({ value: event.target.value });
  };
  render() {
    return (
      <div>
        {" "}
        <h1>Hello React ES6 Class Component!</h1>{" "}
        <input value={this.state.value} type="text" onChange={this.onChange} />{" "}
        <p>{this.state.value}</p>{" "}
      </div>
    );
  }
}
```

使用 `useEffect` 钩子的函数组件可以实现相同的功能，每当指定的状态变化时，就会执行副作用代码，例如更新本地存储的值：

```jsx
const App = () => {
  const [value, setValue] = React.useState(
    localStorage.getItem("myValueInLocalStorage") || ""
  );
  React.useEffect(() => {
    localStorage.setItem("myValueInLocalStorage", value);
  }, [value]);
  const onChange = (event) => setValue(event.target.value);
  return (
    <div>
      {" "}
      <h1>Hello React Function Component!</h1>{" "}
      <input value={value} type="text" onChange={onChange} /> <p>{value}</p>{" "}
    </div>
  );
};
```

函数组件因为可以直接在函数体内使用状态和副作用而显得更加轻量。如果您的下一个组件需要处理副作用，尝试使用带有 React Hooks 的函数组件。

## 通过自定义 React Hooks 实现抽象

我们已经看到了 React 提供的内置 Hooks，但您也可以创建自定义 Hooks 来解决特定问题，这对于可复用的组件逻辑来说非常理想。例如，我们可以将状态管理和本地存储的逻辑封装到一个自定义钩子中：

```jsx
const useStateWithLocalStorage = (localStorageKey) => {
  const [value, setValue] = React.useState(
    localStorage.getItem(localStorageKey) || ""
  );
  React.useEffect(() => {
    localStorage.setItem(localStorageKey, value);
  }, [value]);
  return [value, setValue];
};
const App = () => {
  const [value, setValue] = useStateWithLocalStorage("myValueInLocalStorage");
  const onChange = (event) => setValue(event.target.value);
  return (
    <div>
      {" "}
      <h1>Hello React Function Component!</h1>{" "}
      <input value={value} type="text" onChange={onChange} /> <p>{value}</p>{" "}
    </div>
  );
};
```

`useStateWithLocalStorage` 钩子让我们不仅能进行状态管理，还能将状态与浏览器的本地存储同步。每次组件加载时，如果本地存储中已有值，则使用该值作为状态的初始值。

自定义 Hooks 让可重用的逻辑集中在一个函数中，相比之前散布在类组件中的逻辑，Hooks 提供了更好的封装和复用性。虽然可以通过高阶组件实现类似的抽象层，但使用 Hooks 使逻辑更加集中和清晰。

---

本教程介绍了如何使用 Hooks 将 React 类组件迁移到函数组件，探索状态管理和副作用的新方式。在实现带有状态或副作用的组件时，考虑使用 React 函数组件和 Hooks。React 提供了实现这一目标的所有工具，例如，[在函数组件中使用 Hook 获取数据](https://www.robinwieruch.de/react-hooks-fetch-data/) 是了解 Hooks 概念的好方式。