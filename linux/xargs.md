# xargs

>xargs (short for "extended arguments") is a command on Unix and most Unix-like operating systems used to build and execute commands from standard input. It converts input from standard input into arguments to a command.
xargs（"扩展参数 "的简称）是 Unix 和大多数类 Unix 操作系统上的一条命令，用于从标准输入建立和执行命令。它将来自标准输入的输入转换成命令的参数。

## How to use

```shell
# 清理本地 tag
$ git tag -l | xargs git tag -d
```

## 参考资料
- [unix xargs](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/xargs.html)
- [wiki xargs](https://en.wikipedia.org/wiki/Xargs)