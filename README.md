# regex-vis-lzcapp（Regex Vis）

[Regex Vis](https://github.com/Bowen7/regex-vis)（辅助学习、编写和验证正则的工具）的懒猫微服（LazyCat）打包。

## 模式

- **静态 Web 应用**：`dist/` 为上游最新源码（Vue 3 + Vite）的构建产物，直接打包进 LPK，无 Docker 镜像。
- **git 版本源**：更新上游 = 重新构建 `dist/` 并打 `v*` tag 触发发布（上游自身不发版，package.json 版本恒为 0.1.0）。
- **双商店发布**：官方平台 + 喵喵商店（MiaoMiao private store）。

## 结构

| 文件 | 说明 |
| --- | --- |
| `package.yml` | 包元数据（`cloud.lazycat.app.regex-vis`） |
| `lzc-manifest.yml` | 静态路由：`/` → `file:///lzcapp/pkg/content/` |
| `lzc-build.yml` | 构建配置（`contentdir: ./dist`） |
| `dist/` | 上游源码构建产物 |
| `icon.png` | 图标 |
| `.github/lazycat-action.yml` | [lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 配置 |

## 更新上游

```sh
git clone https://github.com/Bowen7/regex-vis && cd regex-vis
pnpm install && pnpm build   # 产物在 dist/
# 替换本仓库 dist/ 后提交并打 tag：
git tag v0.0.x && git push origin master --tags
```

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `LZC_API_TOKEN` | 懒猫开放平台 PAT（官方商店发布） |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |
