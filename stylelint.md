# stylelint 配置

## 背景

一款样式代码分析工具

## 文档

https://stylelint.io/

## 安装

```bash
pnpm i -D stylelint
```

## 配置

配合 sass, vue， prettier 使用

```bash
pnpm i -D sass stylelint stylelint-config-standard-scss stylelint-config-recommended-vue-scss stylelint-config-prettier-scss
```

配置文件如下

```json
// .stylelintrc.json

{
    "extends": [
        "stylelint-config-standard-scss",
        "stylelint-config-recommended-vue/scss",
        "stylelint-config-prettier-scss"
    ]
}
```

## stylelint，vue 与 vscode 联合使用

在 `.vscode/settings.json` 文件中添加

```json
// .vscode/settings.json
{
  "stylelint.validate": [
      ...,
      // ↓ Add "vue" language.
      "vue"
  ]
}
```
