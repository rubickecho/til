# Why `Const` Can Change

在 js 中，我们都知道 `const` 的作用是定义一个常量，一旦定义后，就不能再修改了。但是，如果我们定义的是一个对象，那么我们就可以修改对象的属性，这是为什么呢？

```js
const name = 'tom';
name = 'jack';
// Uncaught TypeError: Assignment to constant variable.
```

```js
const obj = {
  name: 'obj'
}

obj.name = 'new name'
```

## Assignment（赋值）VS Mutation（变异）

因为在 JS 中， "assignment"（赋值）和 "mutation"（变更）是两个不同的概念

1. Assignment（赋值）：赋值是将一个值分配给一个变量的过程。例如，let a = 1; 或 a = 2; 都是赋值操作。在这些操作中，我们将一个值（1 或 2）分配给变量 a。如果一个变量是用 const 声明的，那么你不能再给它赋值，也就是说，你不能改变它的值

2. Mutation（变更）：变更是改变一个对象或数组的内容的过程。例如，如果你有一个对象 const obj = { a: 1 };，你可以通过 obj.a = 2; 来改变 obj 的属性 a 的值。这是一个变更操作。即使 obj 是一个 const 变量，你仍然可以改变它的属性值，因为这是变更，而不是赋值

`=` 是赋值操作，而 `const` 是用来限制变量的变异操作的，所以当我们说 `const` 声明的变量不能更改，其实说的是不能重新赋值。