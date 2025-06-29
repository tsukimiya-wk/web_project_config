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

---

2025 年 4 月 28 日添加 semantic-release

-   [semantic-release](./semantic-release.md)

## vue3 项目开发依赖

以下命令假设是在一个已经初始化了 git 仓库的 Vue3 项目里执行

非 Vue 项目，去掉第一行的 `eslint-plugin-vue` 和 `.eslintrc.json` 文件相应的配置即可

### 1. 安装命令

2025 年 5 月 25 更新：使用 `pnpm create @eslint/config@latest` 生成配置文件

```bash {.line-numbers}
# pnpm i -D eslint eslint-plugin-vue
pnpm create @eslint/config@latest
pnpm i -D -E prettier
pnpm i -D eslint-config-prettier eslint-plugin-prettier
# pnpm i -D typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
pnpm i -D sass stylelint postcss postcss-html stylelint-config-standard-scss stylelint-config-standard-vue
pnpm i -D lint-staged
pnpm i -D @commitlint/cli @commitlint/config-conventional
pnpm i -D commitizen
pnpm i -D -E cz-conventional-changelog
pnpm dlx husky-init && pnpm i
pnpm i -D semantic-release @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog @semantic-release/npm @semantic-release/git @semantic-release/github
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

2024 年 5 月 5 日更新：使用".czrc"文件来代替"package.json"中的"config"字段

```rc
<!-- .czrc -->
{
    "path": "cz-conventional-changelog"
}
```

#### 2.2 配置 eslint, stylelint, prettier

从 `config` 文件夹下拷贝下列文件到新项目的根目录里

-   .editorconfig
-   ~~.eslintrc.json~~
-   .stylelintrc.json
-   .prettierrc.json
-   .lintstagedrc.json
-   commitlint.config.js
-   .czrc
-   .releaserc
-   tsconfig.json

将 `workflows` 文件夹下的文件都拷贝到 `.github/workflows/` 下

2024 年 5 月 25 日更新：从 ESLint 9.0.0 开始，需要使用 eslint.config.js 文件来配置，使用 `pnpm create @eslint/config@latest` 生成配置文件。

##### 2.2.1 配置 eslint

prettier 配合 eslint 仍需配置。

截止 2025 月 5 月 31 日，默认生成的，有处理 Vue 的 eslint.config.js 存在一定问题，需要修改配置。

```js
// eslint.config.js
import eslintPluginPrettierRecommended from "eslint-plugin-prettier/recommended";

export default defineConfig([
    // Any other config imports go at the top
    eslintPluginPrettierRecommended,
    // 下方为原有配置
    // 将下方配置修改为
    // pluginVue.configs["flat/essential"],
    {
        files: ["**/*.vue"],
        languageOptions: { parserOptions: { parser: tseslint.parser } },
        // 移到这里来
        extends: [pluginVue.configs["flat/essential"]],
    },
]);
```

#### 2.3 配置 husky

2025 年 5 月 25 日更新：从 Node v24 开始，node.js 改变了加载模块的方式，因此 commitlint 的配置文件要么是 commitlint.config.mjs，package.json 声明字段 `"type": "module"`。

实测，必须是 `commitlint.config.mjs` 才行，`"type": "module"` 无法生效

下列钩子的执行顺序为: `pre-commit` -> `prepare-commit-msg` -> `commit-msg`

```bash
# 在提交信息之前，先运行 lint_staged
npx husky add .husky/pre-commit "npx lint-staged"

# 使用 commitlint 检查提交信息
# 必须使用双引号包裹住 $1
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'

# 使用 commitizen 代替 git commit
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx cz --hook || true"
```

2025 年 4 月 28 日更新，上述命令在非交互式环境下执行无法正常执行(可能原因是 commitlint 没有正常的配置文件和需要的插件)，需修改为下列代码

```bash
# 先生成上述文件，然后再修改

# commit-msg
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

if [ -t 1 ]; then
    # 仅在交互式终端中执行
    npx --no -- commitlint --edit "$1"
else
    exit 0
fi


# pre-commit
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

if [ -t 1 ]; then
    # 仅在交互式终端中执行
    npx lint-staged
    # npm test # 如果有测试
else
    exit 0
fi


# prepare-commit-msg
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

if [ -t 1 ]; then
    # 仅在交互式终端中执行
    exec < /dev/tty && npx cz --hook || true
else
    exit 0
fi
```

#### 2.4 配置路径别名

```
更新：在 Vue3 项目中，不需要改变 tsconfig.json 文件，由 Volar 直接接管即可，需要在工作区禁止默认的 TS。

在 VS Code 扩展里，输入 `@builtin typescript-language-features`，选择工作区禁用。

注意：vite.config.ts 里仍需配置 resolve.alias
```

需要分别在 `tsconfig.json` 和 `vite.config.ts` 文件中设置

2025 年 6 月 2 日更新：在 tsconfig.json 设置别名无效，可以新设一个 `tsconfig.alias.json` 文件，在 `tsconfig.app.json` 文件中引入

```json
// 非 Vue 项目: tsconfig.json
// Vue 项目：tsconfig.alias.json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "@/*": ["src/*"]
        }
    }
}
```

然后在 `tsconfig.app.json` 文件中引入

```json
// tsconfig.app.json
"extends": ["@vue/tsconfig/tsconfig.dom.json", "./tsconfig.alias.json"],
// 下面这行是项目自带的，改为上面的方法
// "extends": "@vue/tsconfig/tsconfig.dom.json",
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

```markdown
需要在 tsconfig.app.json 中配置别名的原因

1. tsconfig.json，在使用 Project References 模式时，主 tsconfig.json 的 compilerOptions 配置会被忽略，其只作为项目的协调器，真正的编译配置来自被引用的子配置文件。

2. tsconfig.node.json 只负责编译 vite.config.ts（tsconfig.node.json 配置 "include: ['vite.config.ts']"），而 vite.config.ts 没有导入 src/\* 下的文件。

3. tsconfig.app.json 配置 `include: ['src/**/*.ts', 'src/**/*.tsx', 'src/**/*.vue']`, 在此配置文件中的配置能正确应用到 `src` 文件夹下的文件。

总的来说，是作用范围的问题 "include"，范围没有包含的文件，就不会生效。
```

#### 2.5 自动化生成语义化版本

添加配置文件 `.releaserc`，其在`main/master`分支上生成的版本格式为`v1.0.0`，在`develop`分支上生成的版本格式为`v1.0.0-develop.1`

```json
{
    "branches": [
        "main",
        {
            "name": "develop",
            "prerelease": true
        }
    ],
    "plugins": [
        "@semantic-release/commit-analyzer",
        "@semantic-release/release-notes-generator",
        [
            "@semantic-release/changelog",
            {
                "changelogFile": "CHANGELOG.md"
            }
        ],
        "@semantic-release/npm",
        [
            "@semantic-release/git",
            {
                "assets": ["CHANGELOG.md", "package.json"]
            }
        ],
        "@semantic-release/github"
    ]
}
```

然后在`.github/workflows/semantic-release.yaml`中添加语义化版本的任务。参考 [semantic-release/release.yml](https://github.com/semantic-release/semantic-release/blob/master/.github/workflows/release.yml)

```yaml
name: Release
on:
    push:
        branches:
            - develop
            - main
    pull_request:
        branches:
            - main
permissions:
    contents: write # 允许 checkout
jobs:
    release:
        runs-on: ubuntu-latest
        permissions:
            contents: write # 允许发布到 GitHub release
            issues: write # 允许对已发布的问题发表评论
            pull-requests: write # 允许对已发布的拉取请求发表评论
            id-token: write # 允许将 OIDC 用于 npm 来源
        steps:
            # 1. 检出代码
            - name: Checkout code
              uses: actions/checkout@v4
              with:
                  fetch-depth: 0 # 需要完整历史记录以分析提交信息

            # 2. 设置Node.js环境
            - name: Setup Node.js
              uses: actions/setup-node@v4
              with:
                  node-version: 22

            # 3. 安装依赖
            - name: Install dependencies
              run: npm install

            # 4. 运行 semantic-release
            - name: Run semantic-release
              env:
                  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
              run: npx semantic-release
```
