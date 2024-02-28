# What React is？

- Created: February 21, 2024 10:45 PM
- 模块: React
- 分类: React
- 相关链接: https://learn.react-js.dev/introduction/general
- 学习状态: Yes
- Update: February 22, 2024 10:41 PM

# React 是什么？

> **The library for web and native user interfaces**
> 

是一个前端库而不是前端框架，专注构建用户界面（简单的说就是使开发用户界面更加容易）。

## just a library, not a framework

当我们去开发应用时，随着应用的复杂度提升，会遇到越来越多的问题，而这些问题的解决方案一般不包含在 React 库中，而是由它的周边生态去完成，我们可以按需引入并使用，比如：

1. 复杂的状态管理：Redux
2. 复杂的客户端路由管理：React-Router
3. 项目构建：Webpack、Vite
4. 等等…

而一般我们是把“包含库本身和附加功能”的解决方案成为框架，比如：

- NextJS：The React Framework for the Web，集 SSR、SSG 的服务端框架，内置路由、构建、部署等等多种功能

## 专注构建用户 UI 界面

核心由两部分组成：

1. 基于状态的声明式（Declarative）渲染
2. 组件化的层次结构

### 基于状态的声明式（Declarative）渲染

我们可以把 React 看作 MVC 或者 MVVM 中的 V，即视图层，它在现代前端框架的实现原理

$$
UI = f(state)
$$

- state 代表“当前视图状态”，即当前视图依赖的数据
- f 代表“框架内部运行机制”，即 React 内部核心处理逻辑
- UI 代表“渲染的视图”，即呈现的样子

那么 React 是如何描述 UI 的呢？JSX，[官方文档](https://react.dev/learn/writing-markup-with-jsx)描述

> *JSX* is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file. Although there are other ways to write components, most React developers prefer the conciseness of JSX, and most codebases use it.
> 

当状态变化时，React 会重新计算 UI，这个过程是自动的。我们不需要关心 UI 更新的具体细节，只需要关注状态变化时描述 UI 如何改变。这个动作，也叫做”基于状态的声明式（Declarative）渲染“。

### 组件化的层次结构

组件化（Component）是指将 UI 拆分为独立的、可复用的更小组件（Wiget），每个小组件负责应用的一小部分 UI。一个复杂应用，就是由无数个小组件组合而成。

这些小组件以树状的结构组织，可以嵌套使用，同时又保持每个组件的简单性和专注性。这样也有助于提高代码的复用性和可维护性。

![Vue components](https://cn.vuejs.org/assets/components.dSWW39P2.png)