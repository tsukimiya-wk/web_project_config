# prettier 配置

## 背景

一款代码格式化工具

## 文档

https://prettier.io/docs/en/configuration.html

## 安装

```bash
pnpm i -D -E prettier
```

## 配置

```json
// .prettierrc.json
{
    "printWidth": 80,
    "tabWidth": 4,
    "useTabs": false,
    "semi": true,
    "singleQuote": false,
    "quoteProps": "as-needed",
    "trailingComma": "es5",
    "bracketSpacing": true,
    "bracketSameLine": true,
    "arrowParens": "always",
    "endOfLine": "lf"
}
```

## 其他

-   [eslint](./eslint.md)
-   [stylelint](./stylelint.md)
