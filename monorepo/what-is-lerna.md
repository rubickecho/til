# What is lerna

> Lerna is a fast, modern build system for managing and publishing multiple JavaScript/TypeScript packages from the same repository.
> Lerna是一个快速、现代的构建系统，用于管理和发布来自同一资源库的多个JavaScript/TypeScript包。

***copy form [Lerna Introduction](https://lerna.js.org/docs/introduction)***
It solves three of the biggest problems of JavaScript monorepos:

* Lerna links different projects within the repo, so they can import each other without having to publish anything to NPM.
* Lerna runs a command against any number of projects, and it does it in the most efficient way, in the right order, and with the possibility to distribute that on multiple machines.
* Lerna manages your publishing process, from version management to publishing to NPM, and it provides a variety of options to make sure any workflow can be accommodated.

## 如何安装

```
npm install --global lerna
```

## 如何使用

```
lerna

  A tool for managing JavaScript projects with multiple packages.
  More information: https://lerna.js.org.

  - Initialize project files (lerna.json, package.json, .git, etc.):
    lerna init

  - Install all external dependencies of each package and symlink together local dependencies:
    lerna bootstrap

  - Run a specific script for every package that contains it in its package.json:
    lerna run script

  - Execute an arbitrary shell command in every package:
    lerna exec -- ls

  - Publish all packages that have changed since the last release:
    lerna publish
```

## 参考资料

- [lerna](https://lerna.js.org/)