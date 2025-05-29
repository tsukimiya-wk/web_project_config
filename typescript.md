# typescript 配置

## 背景

一个 ECMAScript 类型的严格超集，可编译为纯 JavaScript

## 文档

https://www.typescriptlang.org/docs/

## 安装

```bash
pnpm i -D typescript
```

## 配置

```bash
npx tsc --init
```

### 路径别名

需要分别在 `tsconfig.json` 和 `vite.config.ts` 文件中设置

```json
// tsconfig.json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "@/*": ["src/*"]
        }
    }
}
```

```ts
// vite.config.ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import path from "path";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [vue()],
    resolve: {
        alias: {
            "@": path.resolve(__dirname, "src"),
        },
    },
});
```

## 与 eslint 联合使用(已过时)

参考 [eslint](./eslint.md)。

```bash
# 安装对应插件 - https://typescript-eslint.io/
pnpm i -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

配置文件如下

```json
// .eslintrc.json
{
    "env": {
        "browser": true,
        "node": true,
        "es2021": true // 支持新语法，但不支持新的全局变量或类型
    },
    "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
    "overrides": [],
    "parser": "@typescript-eslint/parser", // 使用 vue-ts 时，这行放在这里，仅使用 ts 时，放在外边
    "parserOptions": {
        // 指定想要支持的语言选项
        "ecmaVersion": "latest", // 支持新的全局变量或类型
        "sourceType": "module" // 脚本类型，默认为 `script`，`module`表示为模块
        // "ecmaFeatures"， 可选，启用额外的语言特性，如 {"jsx": true}
    },
    "plugins": ["@typescript-eslint"],
    "rules": {}
}
```

## 其他

-   [eslint](./eslint.md)
