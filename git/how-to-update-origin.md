# How to update git origin

## 1. Check current remote origin

```bash
git remote -v
```

## 2. Change remote origin

```bash
# if origin is empty
git remote add origin <new-url>

# if has origin, remove it first
git remote remove origin
git remote add origin <new-url>
```