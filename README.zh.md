# DSH 非官方桌面构建

**非官方构建。与 DeepSeek 无隶属关系，也未获得 DeepSeek 的赞助或背书。**

本仓库发布 **DSH（DeepSeek Harness）** 的桌面安装包，全部直接使用上游开源仓库在指定 tag 或 commit 上的源码，通过上游自带的打包脚本编译生成。

- 上游源码：<https://github.com/deepseek-ai/deepseek-harness>
- 上游许可证：**MIT** — 见 [LICENSE](./LICENSE) 与 [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md)
- 打包方式：使用上游的 `package:desktop:*:unsigned` 脚本（不使用官方签名密钥或官方品牌素材）

## 下载

打开 [Releases](../../releases) 页面，按平台选择：

| 平台 | 文件名格式 | 说明 |
|---|---|---|
| Windows x64 | `DSH-<版本>-win-x64.exe` | 未签名；可能触发 SmartScreen 提示 |
| macOS Apple Silicon | `DSH-<版本>-mac-arm64.dmg` | 未签名；见下方 macOS 说明 |
| macOS Intel | `DSH-<版本>-mac-x64.dmg` | 未签名；见下方 macOS 说明 |

每个 Release 还会附带：

- `SHA256SUMS.txt`
- `LICENSE`
- `THIRD_PARTY_NOTICES.md`

## 校验下载文件

Linux：

```bash
sha256sum -c SHA256SUMS.txt
```

macOS：

```bash
shasum -a 256 -c SHA256SUMS.txt
```

### macOS 未签名构建

这些构建没有使用 Apple Developer ID 签名，也没有经过公证，Gatekeeper 会阻止直接双击打开。可以：

- 右键点击 App → **打开** → 再点一次 **打开**；或
- 把 App 拖到 `/Applications` 后，移除隔离属性：

```bash
xattr -dr com.apple.quarantine "/Applications/<应用名>.app"
```

请只对已经核对过 SHA-256 的构建执行这一步。

## 版本命名

Release tag 使用「上游 tag + 非官方后缀」：

```
dsh-v0.2.3-alpha.2-unofficial.1
```

Release 说明里会记录本次构建使用的上游 commit SHA。除打包外不修改源码；如果以后确实需要打补丁，补丁 diff 和对应源码会在同一个 Release 里一并公开。

## 构建方式

构建在本仓库的 GitHub Actions 中执行，每个平台的任务流程：

1. 检出 `deepseek-ai/deepseek-harness` 的指定 tag/commit；
2. 安装固定版本的 `pnpm` 与 Node.js；
3. 执行 `pnpm install --frozen-lockfile`；
4. 执行 `pnpm run build`；
5. 使用上游对应平台的 `:unsigned` 脚本打包桌面端；
6. 把安装包作为 workflow artifact 上传。

只有把 `publish` 输入设为 `true` 时，发布任务才会创建 GitHub Release。

## 安全说明

- 不内置任何 API Key、Token 或账号凭据；
- 应用连接的是你自己配置的模型服务地址；
- 构建来自公开上游源码，CI 日志公开可查；
- 运行任何安装包前，请先核对 SHA-256。

## 许可证与商标

上游代码采用 MIT License，Copyright (c) 2026 DeepSeek。完整许可证文本已包含在本仓库和每个 Release 中。

“DeepSeek Harness” 是 DeepSeek 的商标。本项目名称使用缩写 **DSH**，不使用官方 Logo，也不暗示官方背书，以符合上游品牌规范；描述性文字（如“基于 DeepSeek Harness 源码构建”）仅用于说明上游来源。

## 问题反馈

构建/打包问题请在本仓库提 Issue；上游应用本身的问题请提交到：

<https://github.com/deepseek-ai/deepseek-harness/issues>
