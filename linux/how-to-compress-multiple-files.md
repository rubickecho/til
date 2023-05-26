# How to compress multiple files

## Introduction

以打包 tar 包为例，假设有如下目录结构：

```bash
.
├── file.txt
├── project-a
├── project-b
└── project-c
```

要将 `file.txt` 和 `project-*` 目录下的所有文件打包成一个 `tar` 文件，可以使用如下命令：

```bash
tar -cvf archive.tar file.txt project-*

# OR

tar -cvf archive.tar ./*
```

要将指定文件或目录打包成 `tar` 文件，可以使用如下命令：

```bash
tar -cvf archive.tar file.txt project-a
```

## References

- [tar command in Linux with examples](https://www.geeksforgeeks.org/tar-command-linux-examples/)