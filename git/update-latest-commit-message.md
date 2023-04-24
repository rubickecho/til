# How to update the latest commit message in git

## Problem
有些时候，我们需要在提交代码的时候，填写本次代码更新的信息，但是可能出现打错字，或者忘记填写的情况，这个时候，我们就需要修改最后一次提交的信息。

## Solution

```bash
git commit --amend
```

## Example

```bash
$ git commit -m "first commit"
```

```bash
$ git commit --amend -m "update first commit"
```