# regex-vis-lzcapp（Regex Vis）

[Regex Vis](https://github.com/Bowen7/regex-vis)（辅助学习、编写和验证正则的工具）的懒猫微服（LazyCat）打包。

## 模式

- **静态 Web 应用**：`dist/` 为上游源码（Vue 3 + Vite）的构建产物，直接打包进 LPK，无 Docker 镜像。
- **自动跟上游**：`sync-upstream.yml` 每日检查上游 `Bowen7/regex-vis` main HEAD（上游无 tag/release，以 commit SHA 为准）；有变化时在 CI 内 `pnpm build` 重建 `dist/`，自动 patch +1 打 `v*` tag 触发发布（`.upstream-sha` 记录已构建的 SHA 基线）。
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

## 更新流程（全自动）

每日 `sync-upstream.yml`（约北京时间 14:41）自动执行；也可手动 Run workflow 立即检查：

1. 对比上游 main HEAD SHA 与 `.upstream-sha`，无变化则跳过；
2. clone 上游 → `pnpm@9 install && build` 重建 `dist/`；
3. `package.yml` 版本自动 patch +1，提交并打 `v*` tag；
4. tag 触发 `lazycat.yml` 构建并发布双商店。

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `LZC_API_TOKEN` | 懒猫开放平台 PAT（官方商店发布） |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |
