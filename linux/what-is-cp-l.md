# Linux "cp -l" 命令介绍

## Why

在 Linux 操作系统中，有时候需要在不同的目录之间复制文件，但是如果使用普通的 "cp" 命令进行复制，会导致多份副本占用过多的磁盘空间。为了避免这种情况，可以使用 "cp -l" 命令来创建硬链接，就可以实现在不同目录之间共享同一份文件的效果。

## What

"cp -l" 命令是 Linux 操作系统下的一个命令行工具，用于创建指向同一文件的硬链接。
它可以将目标文件复制到指定位置，并在指定位置创建一个指向目标文件的新硬链接。
这样，即使在不同的位置打开该文件，实际上打开的都是同一个文件，从而实现文件共享的功能。

## How

使用 "cp -l" 命令的语法格式如下：

```
cp -l [源文件路径] [目标文件路径]

```

其中，"-l" 参数表示创建硬链接。

例如，要将 "/home/user/file.txt" 复制到 "/home/user/documents/" 目录下，并创建一个指向该文件的硬链接，可以使用以下命令：

```
cp -l /home/user/file.txt /home/user/documents/

```

执行该命令后，会在 "/home/user/documents/" 目录下创建一个名为 "file.txt" 的新硬链接，该硬链接指向原始文件 "/home/user/file.txt"，从而实现文件共享的功能。