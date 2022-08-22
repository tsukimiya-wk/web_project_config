# lint-staged 配置

## 背景

在执行提交操作（git commit）前，使用静态代码分析工具（linter）对暂存区内的文件进行检查或格式化

## 文档

https://www.npmjs.com/package/lint-staged

## 安装

```bash
pnpm i -D lint-staged
```

## 配置

```bash
# 配合 husky 使用
npx husky add .husky/pre-commit "npx lint-staged"
```

配合 eslint, stylelint, prettier 等工具，检查暂存区内的文件，并对其进行格式化处理

```json
// .lintstagedrc.json

{
    "*.{scss,vue}": ["stylelint"]
    "*.{js,ts,jsx,tsx,vue}": ["eslint --fix"],
    "*.md": ["prettier --write"]
}
```

## 其他

-   [eslint](./eslint.md)
-   [stylelint](./stylelint.md)
-   [prettier](./prettier.md)
