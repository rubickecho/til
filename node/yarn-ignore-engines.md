# yarn --ignore-engines

## 报错原因

```shell
# 安装依赖
$ yarn
```

yarn 安装依赖时，报错 which@3 需要的 node 版本和当前环境不匹配
>error which@3.0.0: The engine "node" is incompatible with this module. Expected version "^14.17.0 || ^16.13.0 || >=18.0.0". Got "14.16.0"
>error Found incompatible module.

## 如何解决
添加 --ignore-engines，忽略对 node 版本的检查

```shell
$ yarn --ignore-engines
```

## 官方描述

```shell
➜  til git:(main) yarn help | grep -- --ignore
    --ignore-engines                    ignore engines check
    --ignore-optional                   ignore optional dependencies
    --ignore-platform                   ignore platform checks
    --ignore-scripts                    don't run lifecycle scripts
```

### What does --ignore-engines flag do?
copy from [What does --ignore-engines flag do?](https://www.reddit.com/r/node/comments/o9j87e/what_does_ignoreengines_flag_do/)

There's a property in package.json files called engines (https://docs.npmjs.com/cli/v7/configuring-npm/package-json#engines).

It allows package authors to state which node engines are required for the package to work. Basically, which node version is needed.

If you ignore this guidance, you run the risk of installing a package that won't work with the version of node that you ar using because it relies on a feature that your version of node does not support.

## 参考资料
- [yarn-install-ignore-engines](https://classic.yarnpkg.com/lang/en/docs/cli/install/#toc-yarn-install-ignore-engines)
- [The engine "node" is incompatible with this module](https://github.com/gilbarbara/react-joyride/issues/131)

