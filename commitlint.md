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

```js
// commitlint.config.js
module.exports = {
    extends: ["@commitlint/config-conventional"],
};
```

## 配合 husky 使用

```bash
# 使用 commitlint 检查提交信息
npx husky add .husky/commit-msg "npx --no -- commitlint --edit '$1'"
```

## 其他

-   [husky](./husky.md)
-   [commitizen](./commitizen.md)
