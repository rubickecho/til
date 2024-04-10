# React List and Key

## **Why do we need a React List Key?**

### 1. 识别元素以优化渲染

当列表数据发生变化时（比如项目被添加、删除或者重新排序），React 将使用 key 属性来识别每个元素，从而决定如何高效地更新 DOM。通过对比 key，可以直接比较新旧两个列表，识别那些元素是重复的，从而重复这些元素的 DOM 节点，而不是销毁后重新创建，这样可以提高性能。

同样，在 Vue 中也有类似的 key 应用。

### 2. 保持状态和上下文

在列表中，每个元素可能有自己的状态或上下文信息。比如一个列表中的每个条目可能时一个带有编辑 Button 的商品，点击 Button 会弹出一个编辑框。如果没有 key， React 可能会将元素搞混，导致编辑弹窗时显示的时其他商品的信息。

key 可以确保元素的唯一性，从而维护正确的状态和上下文，所以这也是不建议使用 Array Index 作为 key，而是应该保证 key 的唯一性。

### 3. 避免组件状态丢失

在某些情况下，列表中的组件可能拥有本地的状态,比如学习资料中提到的 Checkbox，可能因为列表的更新，导致一个组件的状态错误的应用在另一个组件上。

使用 key 可以帮助 React 跟踪每个组件的身份，确保状态不会错误的迁移到其他组件上。

## React 内部是如何使用 key?

React 内部实现 key 的机制主要是在虚拟 DOM Diff 算法阶段:

1. 生成虚拟 DOM
    
    当组件的状态或属性发生变化时，React 会创建一个新的虚拟 DOM 树。这个新树通过遍历组件 JSX 结构生成，包括所有的元素、属性和 key 值
    
2. 差异计算（Diffing）
    
    将新旧两个虚拟 DOM 树进行比较，这个过程成为 Diff 算法。在这个阶段，会检查每个元素的 key 值，来确定那些元素时新的，那些元素是从旧树中移动过来，以及那些元素被删除了。
    
3. 高效的 DOM 更新
    
    利用 key 值，可以很高效的判断出那些元素是稳定的，即它们在新旧树中的位置没有发生变化。这样可以重用这些元素 DOM 节点，而不需要重新创建它们。对于需要更新或移动的元素，会计算出最小的操作集，最小化 DOM 更改，减小开销。
    
4. 组件的 key 处理
    
    当组件使用 key 属性时，react 会将这个 key 值传递给组件作为它们的 key 属性。
    
    对于函数组件，如果没有明确定义接受 key 属性，key 值会被忽略。
    
    对于类组件，key 值会被设置为 this.props.key
    

## 简化的实现示例

```jsx
// 一个简化的虚拟DOM节点
class VNode {
  constructor(type, props, children) {
    this.type = type;
    this.props = props;
    this.children = children || [];
  }
}

// 一个简化的DOM操作函数
function updateDom(vNode1, vNode2) {
  if (vNode1.type !== vNode2.type) {
    // 如果节点类型不同，直接替换DOM节点
    return createEl(vNode2);
  } else if (vNode1.props.key !== vNode2.props.key) {
    // 如果key不同，也认为是不同的节点，进行替换
    return createEl(vNode2);
  } else {
    // 如果节点类型相同且key相同，比较子节点
    return updateChildren(vNode1, vNode2);
  }
}

// 更新子节点
function updateChildren(vNode1, vNode2) {
  const newChildren = vNode2.children.map(child => updateDom(vNode1.children.find(c => c.props.key === child.props.key), child));
  return newChildren;
}

// 创建DOM节点
function createEl(vNode) {
  const el = document.createElement(vNode.type);
  vNode.children.forEach(child => {
    el.appendChild(createEl(child));
  });
  return el;
}

// 一个简单的虚拟DOM更新示例
const root = document.getElementById('root');

const vNode1 = new VNode('ul', {}, [
  new VNode('li', { key: '1' }, 'Item 1'),
  new VNode('li', { key: '2' }, 'Item 2')
]);

const vNode2 = new VNode('ul', {}, [
  new VNode('li', { key: '2' }, 'Item 2B'),
  new VNode('li', { key: '3' }, 'Item 3')
]);

root.appendChild(createEl(vNode1)); // 初始渲染
updateDom(vNode1, vNode2); // 更新DOM
```

## 涉及源码

- ReactElement
    
    在 `ReactElement` 函数中，创建新的 React 元素时，会将 key 作为参数传入，并将其存储在新创建的元素对象中
    src/packages/react/cjs/react.development.js
    
    ```js
    /**
    * JSX => ReactElement => Fiber
    */
    const ReactElement = function(type, key, ref, self, source, owner, props) {
        const element = {
            // This tag allows us to uniquely identify this as a React Element
            $$typeof: REACT_ELEMENT_TYPE,

            // Built-in properties that belong on the element
            type: type,
            key: key,
            ref: ref,
            props: props,

            // Record the component responsible for creating this element.
            _owner: owner,
        };

        //... 省略

        return element;
    };
    ```
    
- cloneAndReplaceKey
    
    在 `cloneAndReplaceKey` 函数中，会创建一个新的 React 元素，并使用新的 key 替换旧的 key。
    
    src/packages/react/cjs/react.development.js
    
    ```jsx
    function cloneAndReplaceKey(oldElement, newKey) {
      var newElement = ReactElement(oldElement.type, newKey, oldElement.ref, oldElement._self, oldElement._source, oldElement._owner, oldElement.props);
      return newElement;
    }
    ```
    
- cloneElement
    
    在 `cloneElement` 函数中，如果传入的 config 对象中包含有效的 key，那么会使用这个新的 key 替换原有元素的 key。
    
    ```jsx
    /**
     * Clone and return a new ReactElement using element as the starting point.
     * See https://reactjs.org/docs/react-api.html#cloneelement
     */
    
    function cloneElement(element, config, children) {
      if (element === null || element === undefined) {
        throw new Error("React.cloneElement(...): The argument must be a React element, but you passed " + element + ".");
      }
    
      var propName; // Original props are copied
    
      var props = _assign({}, element.props); // Reserved names are extracted
    
      var key = element.key;
      var ref = element.ref; // Self is preserved since the owner is preserved.
    
      var self = element._self; // Source is preserved since cloneElement is unlikely to be targeted by a
      // transpiler, and the original source is probably a better indicator of the
      // true owner.
    
      var source = element._source; // Owner will be preserved, unless ref is overridden
    
      var owner = element._owner;
    
      if (config != null) {
        if (hasValidRef(config)) {
          // Silently steal the ref from the parent.
          ref = config.ref;
          owner = ReactCurrentOwner.current;
        }
    
        if (hasValidKey(config)) {
          {
            checkKeyStringCoercion(config.key);
          }
    
          key = '' + config.key;
        } // Remaining properties override existing props
    
        var defaultProps;
    
        if (element.type && element.type.defaultProps) {
          defaultProps = element.type.defaultProps;
        }
    
        for (propName in config) {
          if (hasOwnProperty.call(config, propName) && !RESERVED_PROPS.hasOwnProperty(propName)) {
            if (config[propName] === undefined && defaultProps !== undefined) {
              // Resolve default props
              props[propName] = defaultProps[propName];
            } else {
              props[propName] = config[propName];
            }
          }
        }
      } // Children can be more than one argument, and those are transferred onto
      // the newly allocated props object.
    
      var childrenLength = arguments.length - 2;
    
      if (childrenLength === 1) {
        props.children = children;
      } else if (childrenLength > 1) {
        var childArray = Array(childrenLength);
    
        for (var i = 0; i < childrenLength; i++) {
          childArray[i] = arguments[i + 2];
        }
    
        props.children = childArray;
      }
    
      return ReactElement(element.type, key, ref, self, source, owner, props);
    }
    ```
    
- checkKeyStringCoercion
    
    在 `checkKeyStringCoercion` 函数中，会检查 key 是否可以被强制转换为字符串，如果不能，则会抛出错误。
    
    ```jsx
    function testStringCoercion(value) {
      // If you ended up here by following an exception call stack, here's what's
      // happened: you supplied an object or symbol value to React (as a prop, key,
      // DOM attribute, CSS property, string ref, etc.) and when React tried to
      // coerce it to a string using `'' + value`, an exception was thrown.
      //
      // The most common types that will cause this exception are `Symbol` instances
      // and Temporal objects like `Temporal.Instant`. But any object that has a
      // `valueOf` or `[Symbol.toPrimitive]` method that throws will also cause this
      // exception. (Library authors do this to prevent users from using built-in
      // numeric operators like `+` or comparison operators like `>=` because custom
      // methods are needed to perform accurate arithmetic or comparison.)
      //
      // To fix the problem, coerce this object or symbol value to a string before
      // passing it to React. The most reliable way is usually `String(value)`.
      //
      // To find which value is throwing, check the browser or debugger console.
      // Before this exception was thrown, there should be `console.error` output
      // that shows the type (Symbol, Temporal.PlainDate, etc.) that caused the
      // problem and how that type was used: key, atrribute, input value prop, etc.
      // In most cases, this console output also shows the component and its
      // ancestor components where the exception happened.
      //
      // eslint-disable-next-line react-internal/safe-string-coercion
      return '' + value;
    }
    function checkKeyStringCoercion(value) {
      {
        if (willCoercionThrow(value)) {
          error('The provided key is an unsupported type %s.' + ' This value must be coerced to a string before before using it here.', typeName(value));
    
          return testStringCoercion(value); // throw (to help callers find troubleshooting comments)
        }
      }
    }
    ```
    
- hasValidKey
    
    在 `hasValidKey` 函数中，会检查传入的 config 对象中是否包含有效的 key。
    
    ```jsx
    function hasValidKey(config) {
      {
        if (hasOwnProperty.call(config, 'key')) {
          var getter = Object.getOwnPropertyDescriptor(config, 'key').get;
    
          if (getter && getter.isReactWarning) {
            return false;
          }
        }
      }
    
      return config.key !== undefined;
    }
    ```
    
## 最佳实践

1. key 应该是唯一的：在同一层级的 React 元素中，key 应该是唯一的。这样可以帮助 React 识别哪些元素发生了变化，哪些元素被添加或删除。
2. key 不应该是随机生成的：每次重新渲染时，key 都应该保持不变。如果 key 是随机生成的，那么每次重新渲染时，React 都会认为所有元素都是新元素，这会导致重新渲染的性能下降。
3. key 不应该依赖于数组索引：尽管使用数组索引作为 key 是一个简单的解决方案，但是如果列表可以重新排序或修改，这可能会导致性能问题或状态错误。因为 key 是 React 用来识别元素的方式，如果 key 是数组索引，那么重新排序列表时，元素的 key 会改变，这可能会导致 React 错误地复用元素。
4. key 应该在兄弟元素之间是唯一的，而不需要在全局是唯一的：React 只需要在当前的兄弟元素之间唯一。key 也不需要在组件之间是唯一的。
5. key 不能在函数组件中被访问：key 和 ref 是 React 的保留属性，它们不能作为 props 传递给子组件。如果你需要在子组件中使用 key，可以将其作为一个不同的 prop 传递。