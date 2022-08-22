# husky 配置

## 背景

一个使 Git hooks 更易用的开发工具

## 文档

https://typicode.github.io/husky

## 安装

```bash
# 推荐自动安装
# 需要在一个 git 仓库里才能正常应用

git init
pnpm dlx husky-init && pnpm i
```

## 配置

添加钩子

```bash
# 在提交信息之前，先运行 lint_staged
npx husky add .husky/pre-commit "npx lint-staged"

# 使用 commitlint 检查提交信息
npx husky add .husky/commit-msg "npx --no -- commitlint --edit '$1'"

# 使用 commitizen 代替 git commit
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx cz --hook || true"
```

## 其他

-   [lint-staged](./lint_staged.md)
-   [commitlint](./commitlint.md)
-   [commitizen](./commitizen.md)
