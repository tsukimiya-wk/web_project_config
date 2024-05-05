# commitizen 配置

## 背景

一款生成符合 Git Commit 规范信息的工具

## 文档

github.com/commitizen/cz-cli

## 安装

```bash
pnpm i -D commitizen
```

## 配置

Note:

使用 pnpm 安装的 commitizen 无法正确执行下列命令

```bash
commitizen init cz-conventional-changelog --save-dev --save-exact
```

需要手动配置

1. 安装 `cz-conventional-changelog`
2. 將其保存在 `package.json` 文件的 `dependencies` 或 `devDependencies` 字段里
3. 在 `package.json` 文件中添加 `config.commitizen` 字段

```bash
pnpm i -D -E cz-conventional-changelog
```

```json
// package.json
{
    "scripts": {
        "cm": "cz"
    }
    "config": {
        "commitizen": {
            "path": "cz-conventional-changelog"
        }
    }
}
```

2024 年 5 月 5 日更新：使用".czrc"文件来代替"package.json"中的"config"字段

```
// .czrc
{
    "path": "cz-conventional-changelog"
}
```

使用 `npm run cm` 代替 `git commit` 命令

运行 `npm run cm`，根据提示生成符合 commit 规范的提交信息

Note:

使用 husky 的 precommit 钩子时，脚本命令不能是 `commit`，因为 npm 会自动执行名为 `prexxx` 的命令，其中 `xxx` 是另一个脚本名，如果脚本名为 `commit`，npm 和 husky 将会运行两次 `precommit` 命令

## 配合 husky 使用

```bash
# 使用 commitizen 代替 git commit
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx cz --hook || true"
```

## 配合 husky， commitlint 使用

使用 commitizen 生成信息

使用 husky + commitlint 检查提交信息是否符合规范，防止提交不规范的信息

## 其他

-   [husky](./husky.md)
-   [commitlint](./commitlint.md)
