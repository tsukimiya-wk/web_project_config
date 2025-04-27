# stylelint 配置

## 背景

一款样式代码分析工具

## 文档

https://stylelint.io/

https://www.npmjs.com/package/stylelint-config-standard-vue

https://www.npmjs.com/package/stylelint-config-prettier

## 安装

```bash
pnpm i -D stylelint
```

## 配置

配合 sass, vue， prettier 使用

```bash
pnpm i -D sass stylelint postcss postcss-html stylelint-config-standard-scss stylelint-config-standard-vue
```

配置文件如下

```json
// .stylelintrc.json

{
    "extends": [
        "stylelint-config-standard-scss",
        "stylelint-config-standard-vue/scss"
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

2024 年 5 月 5 日更新：删除了"stylelint-config-prettier"，[stylelint≥15 已废弃了该插件](https://stylelint.io/migration-guide/to-15#deprecated-stylistic-rules)
