# commitlint 配置

## 背景

一款检查 git 提交信息的工具

## 文档

https://commitlint.js.org/

## 安装

```bash
pnpm i -D @commitlint/cli
```

## 配置

先安装对应的包

```bash
pnpm i -D @commitlint/config-conventional
```

配置文件如下

2025 年 5 月 25 更新：将 commonjs 改为 ES 模块

```js
// commitlint.config.mjs
// module.exports = {
//     extends: ["@commitlint/config-conventional"],
// };
export default {
    extends: ["@commitlint/config-conventional"],
};
```

## 报错

必须是 `mjs`

> Node v24 改变了模块的加载方式，这包括 commitlint 配置文件。如果你的项目中不包含 package.文件，commitlint 可能无法加载配置，导致出现“请向您的 commitlint.config.js 文件中添加规则”的错误信息。可以通过以下两种方式之一来修复这个问题：
>
> 添加一个 package.文件，将您的项目声明为 ES6 模块。这可以通过运行 npm init es6 轻松完成。
>
> 将配置文件从 commitlint.config.js 重命名为 commitlint.config.mjs。

## 配合 husky 使用

```bash
# 使用 commitlint 检查提交信息
npx husky add .husky/commit-msg "npx --no -- commitlint --edit '$1'"
```

## 其他

-   [husky](./husky.md)
-   [commitizen](./commitizen.md)
