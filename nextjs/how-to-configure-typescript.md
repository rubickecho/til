# How to configure TypeScript in Next.js

下面介绍如何在 Next.js 应用程序中配置 TypeScript。

## 步骤

1. 在 Next.js 应用程序中安装 TypeScript。

```
npm install --save-dev typescript @types/react @types/node

```
2. 创建一个 `tsconfig.json` 文件，在文件中添加以下内容：

```
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "noEmit": true
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}

```
3. 修改 `next.config.js` 文件，并在文件中添加以下内容：

```
module.exports = {
  /* config options here */
  webpack(config) {
    config.resolve.extensions.push('.ts', '.tsx');
    return config;
  }
}

```
4. 最后，将 `.js` 文件更改为 `.tsx` 文件。

DONE! 享受 TS 的强大功能吧！
