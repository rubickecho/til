# How to use tree to view file directory structure

如何使用 tree 命令查看文件目录结构？

## 如何安装

```bash
# macos
brew install tree
```

## 命令格式

```bash
tree [options] [dir]
```

## 命令参数

```bash
-a 显示所有文件和目录。
-d 显示目录名称而非内容。
-L n 最大显示深度为 n 的目录。
-f 显示完整路径名称。
-i 不显示树形符号。
-p 显示权限。
-P pattern 只显示符合 pattern 的文件或目录。
-s 显示文件或目录的大小。
-u 显示文件或目录的拥有者。
```

## 命令实例

```bash
tree -L 1 -d ./
```

## 命令输出

```bash
.
├── Applications
├── Desktop
├── Documents
├── Downloads
├── Library
├── Movies
├── Music
├── Parallels
├── Pictures
├── Public
```