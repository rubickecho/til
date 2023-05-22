# __dirname 指的是那个路径

在实际项目开发中，__dirname 一般用于获取当前文件所在的目录名，常用于读取本地文件。
也可以获取项目的相对路径，方便我们引入其他模块。

## Example

假如有这样一个文件结构：

```bash
project
├── subproject
│   └── sub.js
└── index.js
```

- 在 `sub.js` 中，`__dirname` 的值是 `/project/subproject`
- 在 `index.js` 中，`__dirname` 的值是 `/project`