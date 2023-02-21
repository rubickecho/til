# Babel loose mode

## what is babel loose mode?

babel loose mode is a babel option that allows you to compile your code in a way that is more compatible with older browsers.

many of the es6 features that babel supports are not supported by older browsers. for example, the spread operator is not supported by older browsers.

many plugin options in babel have a loose mode. for example, the es2015 preset has a loose mode for the es2015 classes plugin.

copy from [Babel 6: loose mode](https://2ality.com/2015/12/babel6-loose-mode.html) :

```
Many plugins in Babel have two modes:

A normal mode follows the semantics of ECMAScript 6 as closely as possible.
A loose mode produces simpler ES5 code.
Normally, it is recommended to not use loose mode. The pros and cons are:

Pros: The generated code is potentially faster and more compatible with older engines. It also tends to be cleaner, more “ES5-style”.
Con: You risk getting problems later on, when you switch from transpiled ES6 to native ES6. That is rarely a risk worth taking.
```

## why is it useful?

babel loose mode is useful when you want to compile your code to es5, but you want to support older browsers.

## examples

### es2015 classes

```js
// This is a class that describes a Foo.
class Foo {
  constructor() {
    // A Foo has a property named bar.
    this.bar = 'bar';
  }
}
```

#### normal mode

```js
var Foo = function Foo() {
  _classCallCheck(this, Foo);

  this.bar = 'bar';
};
```

#### loose mode
```js
class Foo {
  constructor() {
    this.bar = 'bar';
  }
}
```

