# semantic-release 配置

## 背景

一款自动化发布语义版本号的工具

## 文档

https://github.com/semantic-release/semantic-release

## 安装

```bash
pnpm i -D semantic-release @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog @semantic-release/git @semantic-release/github
```

如果要发布到 npm，还需要安装 `@semantic-release/npm`

## 配置

```json
// .releaserc
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

## 其他

-   [husky](./husky.md)
