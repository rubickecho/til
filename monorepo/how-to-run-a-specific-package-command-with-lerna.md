# How to run a specific package command with lerna.

## Description 

如果有下方结构的项目，我期望只构建 component
```shell
./packages
├── @app
│   └── my-app
├── @component
│   └── my-component
```

## How to solve

通过 `--scope` 指定匹配 package

```shell
# "my-component" is package.json name
> npx lerna run --scope 'my-component' --stream build
```

## More information

`scope` 常用匹配方式:

1.包名称：匹配单个包。例如，只需要过滤某个特定的包，那么可以这样使用：
```shell
> lerna run test --scope my-package
```

2.通配符：匹配多个包。例如，* 代表所有包，foo-* 匹配所有包名以 foo- 开头的包。例如：
```shell
> lerna run test --scope '*'
> lerna run test --scope 'foo-*'
```

3.正则表达式：匹配满足正则表达式规则的包。例如，使用正则匹配库名开头一致的包：

```shell
> lerna run test --scope '{package-1,package-2}'
```