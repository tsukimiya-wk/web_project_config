# 通用开发环境的配置

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

## 通用开发依赖

-   [husky](./husky.md)
-   [lint-staged](./lint_staged.md)
-   [commitlint](./commitlint.md)
-   [commitizen](./commitizen.md)
-   [eslint](./eslint.md)
-   [stylelint](./stylelint.md)
-   [prettier](./prettier.md)
-   [typescript](./typescript.md)

## vue3 项目开发依赖

以下命令假设是在一个已经初始化了 git 仓库的 Vue3 项目里执行

非 Vue 项目，去掉第一行的 `eslint-plugin-vue` 和 `.eslintrc.json` 文件相应的配置即可

### 1. 安装命令

```bash {.line-numbers}
pnpm i -D eslint eslint-plugin-vue
pnpm i -D -E prettier
pnpm i -D eslint-config-prettier eslint-plugin-prettier
pnpm i -D typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
pnpm i -D sass stylelint postcss postcss-html stylelint-config-standard-scss stylelint-config-standard-vue stylelint-config-prettier
pnpm i -D lint-staged
pnpm i -D @commitlint/cli @commitlint/config-conventional
pnpm i -D commitizen
pnpm i -D -E cz-conventional-changelog
pnpm dlx husky-init && pnpm i
```

注意：

命令 `pnpm dlx husky-init` 无法在 MacOS 创建文件夹，可以先手动创建 `.husky` 文件夹，再执行该命令

### 2. 生成配置文件

#### 2.1 配置 commitizen

在 `package.json` 文件中添加下列字段

```json
// package.json
{
    "config": {
        "commitizen": {
            "path": "cz-conventional-changelog"
        }
    }
}
```

#### 2.2 配置 eslint, stylelint, prettier

从 `config` 文件夹下拷贝下列 9 个文件到新项目的根目录里

-   .editorconfig
-   .eslintrc.json
-   .eslintignore
-   .stylelintrc.json
-   .stylelintignore
-   .prettierrc.json
-   .prettierignore
-   .lintstagedrc.json
-   commitlint.config.js

#### 2.3 配置 husky

```bash
# 在提交信息之前，先运行 lint_staged
npx husky add .husky/pre-commit "npx lint-staged"

# 使用 commitlint 检查提交信息
# 必须使用双引号包裹住 $1
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'

# 使用 commitizen 代替 git commit
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx cz --hook || true"
```

#### 2.4 配置路径别名

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
