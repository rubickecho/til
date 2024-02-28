# React `Element`、`Component`、`Instance` introduce

- Created: February 23, 2024 11:11 
- 模块: React
- 分类: React
- 相关链接: https://www.robinwieruch.de/react-element-component/
- 学习状态: Yes
- Update: February 26, 2024 9:46 PM

> While a React Component is the one time declaration of a component, it can be used once or multiple times as React Element in JSX. In JSX it can be used with angle brackets, however, under the hood React's `createElement` method kicks in to create React elements as JavaScript object for each HTML element.
> 

从代码片段看

[Babel Compiler Demo](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=JYWwDg9gTgLgBAbzgVwM4FMDKMCGN1wC-cAZlBCHAORTo4DGMVA3AFCiSzUB0A9AIJgw3eqlQtWrXrzj0KkAHboFMVnIWp4ACXQAbXRADq0XQBM4AXjgAKMOTCoAlJYB8iVnDjS4e9CGWqnrQwyFAKcAA8YC4AFsCIdhAO3Ao4_oQANHAxegZwAO4mphG80WyEbKwkyAqMwBDhgmDWzggesg2acADacjUwWRgwAMIQ_QC6ligY2Hjo1gAMjpWe3sAauLXo7epdOfpGRQCSGzhbAIxTEToHxlBmcKn-FgBEACoUL14ubJ678PsDHczCdNGd6OgAExXG5AoqPNLoV4AKQYAGsvrwfpI_p0ILp0NwDABzaxUQGHe6mUGbCHnABcVCyFOB1NOF2WOzxBKJEFJ5NylJB7IhkMZzMFrJp4KhnPawVC4Ws7U8ERcKs8iF4ACofAT_Co4NreIQNarYULzE8kS8ADLIegAT0x6rNWt1tAUpnQUDg6zBWyNJrdCBZxxF6HOps1mtDkvDAdFppDOrgpmAqD6UBwxPQpgAhIWg9GY4gLazrEhrfTqPhNFQiI4S6qse1Ocn0AAPTjwb0kHDIXTwJrMIA&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=true&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=true&targets=&version=7.23.10&externalPlugins=&assumptions=%7B%7D)

```jsx
import { useState } from 'react';
import './App.css';

// component
const HelloWorld = (props) => {
  // element
  return <p>hi {props.name}, hello world</p>;
};

function App() {
  const [count, setCount] = useState(0);

  // instance
  const helloWorldInstance1 = <HelloWorld name="Tom" />;
  const helloWorldInstance2 = <HelloWorld name="Jack" />;

  console.log('helloWorldInstance1:', helloWorldInstance1);
  console.log('helloWorldInstance2:', helloWorldInstance2);

  return (
    <>
      {/* element */}
      <HelloWorld name="Lucy" />

      {/* render instance */}
      {helloWorldInstance1}
      {helloWorldInstance2}

      {/* discouraged!!! */}
      {HelloWorld({ name: 'test' })}
    </>
  );
}

export default App;
```

为什么不建议直接调用组件方法，例如下面这段代码

```jsx
import { useState } from 'react';
import './App.css';

// component
const ChildComponent = (props) => {
  let [count, setCount] = useState(props.defaultCount || 0);
  // element
  return (
    <div style={{ border: '1px solid red', padding: 8 }}>
      <p>
        hi {props.name}, count is {count}
      </p>
      <button onClick={() => setCount(count++)}>Click Me!</button>
    </div>
  );
};

function Discouraged() {
  let [visible, setVisible] = useState(true);

  // instance
  const childInstance1 = <ChildComponent name="Tom" defaultCount={1} />;

  return (
    <>
      {childInstance1}
      <ChildComponent name="Jack" defaultCount={1} />
      {/* discouraged!!! */}
      {ChildComponent({ name: 'test1', defaultCount: 1 })}
      {ChildComponent({ name: 'test2', defaultCount: 1 })}
			
      {visible ? ChildComponent({ name: 'test3', defaultCount: 1 }) : null}

      <button onClick={() => setVisible(!visible)}>
        {visible ? 'Hidden Test3' : 'Show Test3'}
      </button>
    </>
  );
}

export default Discouraged;
```

点击 button，隐藏 Test3，会收到报错

```jsx
Uncaught Error: Rendered fewer hooks than expected. This may be caused by an accidental early return statement.
```

这是因为在 React 组件的渲染过程中，实际运行的 Hooks 数量少于 React 预期运行的 Hooks 数量。在 React 中，不能在条件语句或循环中调用 Hooks，它们必须始终在最顶层调用，并且每次调用时都应保证 Hooks 的数量相同。

要理解上述代码为何会导致 Hook 数量不一致，我们需要从 React JSX 渲染的角度进行分析。

[Babel Compiler Demo](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=JYWwDg9gTgLgBAbzgVwM4FMDKMCGN1wC-cAZlBCHAORTo4DGMVA3AFCiSzUB0A9AIJgw3eqlQtWrXrzj0KkAHboFMVnIWp4AYQAWwADYATLfIhKVcALxwAFGHJhUASisA-RKzhx96eAG05ZBUAGjgMGBMgmABdKxQMbDx0OwdUbkN0EhxkfQiIKLgAH0K4AAYnNi9pOHQfEGVVL1oYZCgFW08vOAAeQ2AANzCYAE8fSwQkACNoDKgALmoARjAADzCIfWBDOFpDKlCwHEM-hQBzBYAOIkJXTq6esFv7-71EewhHbgUcesJQwIswFQiABMEIdy63V4jwhXm6k2QMBgZjgZi0m3oAGtxjYXJZ3OFIiobKCANSkpw3dHALFwACy6AAhFCEUizE9Ibw-v0ORVWIQ2KwSEFGMAUQARIGBKA4U7oQy4jxeHz-fpA4CTHyhcIANXVmvQsWsaCwuHwNhgUGQ6D5nWqwA0uAU9HQnXUmlkeiMAElHThnehFnFuroDMZTOZ4N96pYAEQAFQosbgGSyOTyUXGi2IvFcgqavla7RsEO6HPuCHoXsMvs0_pd2dhPVDRhM4DMDTg0fQcYAUgxMcnU9lckSYFmc-WugheAAqFNS_IyuWGRlruCz3jg56IFvh9uRmxIbsLKj4TSLfYpzIjjMqBZBwiUpsIPdtxQNI9dn7oU_nmAAExXsO6Zjg-RDPgAkNBL5qqgGo-HAAD8cBvhGn7Hj-f7oJoADMwE3qB-T3nAj4uAsCg5Po4JNvCiLIu0aIYtiCCKviYS-Hq8EGjYjJwQhNo3E2XgIPxBrIdQAASWwZO08Y4TA-FwKemA6BAADucDyXhVDbs8LL0eypa5p0fI0egKycPAIG5HAkqoNKsryswQA&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=true&targets=&version=7.23.10&externalPlugins=&assumptions=%7B%7D)，编译后代码如下：

```jsx
import { useState } from "react";
import "./App.css";

// component
const ChildComponent = (props) => {
  let [count, setCount] = useState(props.defaultCount || 0);
  // element
  return /*#__PURE__*/ React.createElement(
    "div",
    {
      style: {
        border: "1px solid red",
        padding: 8
      }
    },
    /*#__PURE__*/ React.createElement(
      "p",
      null,
      "hi ",
      props.name,
      ", count is ",
      count
    ),
    /*#__PURE__*/ React.createElement(
      "button",
      {
        onClick: () => setCount(count++)
      },
      "Click Me!"
    )
  );
};
function Discouraged() {
  let [visible, setVisible] = useState(true);

  // instance
  const childInstance1 = /*#__PURE__*/ React.createElement(ChildComponent, {
    name: "Tom",
    defaultCount: 1
  });
  return /*#__PURE__*/ React.createElement(
    React.Fragment,
    null,
    childInstance1,
    /*#__PURE__*/ React.createElement(ChildComponent, {
      name: "Jack",
      defaultCount: 1
    }),
    ChildComponent({
      name: "test1",
      defaultCount: 1
    }),
    ChildComponent({
      name: "test2",
      defaultCount: 1
    }),
    visible
      ? ChildComponent({
          name: "test3",
          defaultCount: 1
        })
      : null,
    /*#__PURE__*/ React.createElement(
      "button",
      {
        onClick: () => setVisible(!visible)
      },
      visible ? "Hidden Test3" : "Show Test3"
    )
  );
}
export default Discouraged;
```

当使用 JSX 渲染子组件时，React 会将其视为 Element，并隔离其内部实现。而当使用函数的方式渲染时，React 不会将其视为子组件，而是将其所有实现细节（例如 Hooks）都包含在父组件中。因此，当 visible 变化时，会导致父组件的 Hooks 数量发生变化，这违反了 React Hook 的渲染原则，因此会抛出错误。