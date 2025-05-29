# eslint 配置

## 背景

检查 ECMAScript 文件的风格和语法错误的静态代码分析工具

## 文档

https://eslint.org/

## 新安装和配置

从 ESLint 9.0.0 开始，`.eslintrc.json` 已无法使用，需要使用 `pnpm create @eslint/config@latest` 生成新的配置文件 eslint.config.js，无需手动配置

```bash
pnpm create @eslint/config@latest
```

prettier 配合 eslint 仍需配置

```js
// eslint.config.js
import eslintPluginPrettierRecommended from "eslint-plugin-prettier/recommended";

export default defineConfig([
    // Any other config imports go at the top
    eslintPluginPrettierRecommended,
    // 下方为原有配置
]);
```

---

注意：下方的配置均已过时。

## 安装（已过时）

```bash
pnpm i -D eslint
```

## 配置（已过时）

```bash
# 使用 @eslint/create-config 来生成配置文件
pnpm dlx @eslint/create-config

# 使用 prettier 来格式化文件时，选择第二项 - 检查风格与错误
```

配合 prettier 使用

```bash
pnpm i -D prettier eslint-config-prettier eslint-plugin-prettier
```

配合 typescript 使用

```bash
pnpm i -D typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

仅使用 eslint, prettier, typescript 的配置如下

```json
// .eslintrc.json

{
    "env": {
        "browser": true,
        "node": true,
        "es2021": true // 支持新语法，但不支持新的全局变量或类型
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:prettier/recommended"
    ],
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

---

配合 vue 使用（已过时）

```bash
pnpm i -D eslint-plugin-vue
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
    "extends": [
        "eslint:recommended",
        "plugin:vue/vue3-recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:prettier/recommended"
    ],
    "overrides": [],
    "parser": "vue-eslint-parser", // 使用 vue 时需要使用这行
    "parserOptions": {
        // 指定想要支持的语言选项
        "parser": "@typescript-eslint/parser", // 使用 vue-ts 时，这行放在这里，仅使用 ts 时，放在外边
        "ecmaVersion": "latest", // 支持新的全局变量或类型
        "sourceType": "module" // 脚本类型，默认为 `script`，`module`表示为模块
        // "ecmaFeatures"， 可选，启用额外的语言特性，如 {"jsx": true}
    },
    "plugins": ["vue", "@typescript-eslint"],
    "rules": {}
}
```

## 其他

-   [prettier](./prettier.md)
-   [typescript](./typescript.md)
