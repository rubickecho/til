# How to clone target branch in Git

## Introduction
if you want to clone a specific branch of a Git repository, you can use the git clone command and specify the branch name with the -b parameter.

The base command to clone a Git repository is:

```bash
git clone -b <branch-name> <repository-url>
```

- `branch-name`: branch name to clone.
- `repository-url`: URL of the git repository.

## Example

For example to clone a branch named `develop`, you can use the following command:

```bash
git clone -b develop https://github.com/example/repo.git
```

## Other

if you want to see the list of available branches in a remote repository, you can use:

```bash
git ls-remote --heads <repository-url>
```