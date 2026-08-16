<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="Rebuild GitLens - 自动同步上游 GitLens release、应用本地补丁并打包成可安装 vsix 的 GitHub Actions 工作流">
</p>

<h1 align="center">Rebuild GitLens</h1>

<p align="center">
  <a href="https://github.com/aliveranme/Rebuild-gitlens/releases"><img src="https://img.shields.io/github/v/release/aliveranme/Rebuild-gitlens?color=2ea44f&logo=github" alt="Latest Release"></a>
  <a href="https://github.com/aliveranme/Rebuild-gitlens/actions/workflows/build.yml"><img src="https://github.com/aliveranme/Rebuild-gitlens/actions/workflows/build.yml/badge.svg" alt="Build Status"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT"></a>
</p>

<p align="center">用于个人使用及测试的 GitLens 自动构建工作流，支持自动同步上游最新发布并编译打包</p>

---

## 📖 这是什么

一个 GitHub Actions 工作流，自动同步 [gitkraken/vscode-gitlens](https://github.com/gitkraken/vscode-gitlens) 的最新 release，应用 `patches/` 目录下的本地补丁，构建出可直接安装的 `.vsix` 文件及 SHA-256 校验和，并自动发布到本仓库的 [Releases](https://github.com/aliveranme/Rebuild-gitlens/releases)。

## ⚙️ 工作流机制

1. **同步上游版本** — 从 `gitkraken/vscode-gitlens` 拉取指定 tag 或最新 release。
2. **应用本地补丁** — 严格校验并应用 `patches/*.patch`，具备 Fail-Fast 异常中断机制。
3. **构建扩展包** — 执行 `pnpm install`、`oxfmt` 代码规范化与 `pnpm package` 编译。
4. **发布 Release** — 计算 SHA-256 校验和，上传 `.vsix`、`.sha256` 与上游 CHANGELOG。

## 🚀 触发方式

### 手动触发

进入仓库 **Actions → Fix gitlens and build**，点击 **Run workflow**：

- **指定版本**：输入版本号（如 `v19.0.1`）即可定向同步并编译该版本
- **最新版本**：留空则自动检测并拉取上游最新的正式 release

### 定时触发

默认每天 UTC 0:00 自动触发检查与构建：

```yaml
schedule:
  - cron: '0 0 * * *'
```

## 📦 输出产物

- **`.vsix` 安装包**：`gitlens-{version}.vsix`
- **SHA-256 校验和**：`gitlens-{version}.vsix.sha256`
- **GitHub Release**：包含安装文件、校验和与上游完整变更日志

## 🛠️ 补丁特性说明

`patches/` 目录下的补丁会在构建前自动应用：

| 补丁 | 作用说明 |
|------|------|
| `fix-checkin.patch` | 调整登录用户的 Check-in 订阅校验返回值，保留真实个人信息并自动提升至 Enterprise 级别 |
| `fix-subscription.patch` | 适配 GitLens 19 全新架构，解除 Commit Graph 强制登录限制（Bypass sign-in），注入默认已验证企业账户（`Voyager`）支持离线与免登录使用 |

## 💻 快速安装指南

### 命令行一键安装

```bash
# VS Code
code --install-extension gitlens-*.vsix

# Cursor
cursor --install-extension gitlens-*.vsix

# VSCodium
codium --install-extension gitlens-*.vsix
```

### 图形界面安装

1. 打开编辑器侧边栏 **Extensions（扩展）** (快捷键 `Ctrl+Shift+X` 或 `Cmd+Shift+X`)。
2. 点击扩展面板右上角的 **`···` (Views and More Actions)**。
3. 选择 **Install from VSIX...（从 VSIX 安装...）**，选择下载的 `.vsix` 文件即可。

## 🌐 兼容性支持

| 编辑器 / 平台 | 兼容状态 | 说明 |
|---|---|---|
| **VS Code** | 兼容 | 原生支持 |
| **Cursor** | 兼容 | 支持免登录及全部基础/高级特性 |
| **Trae** | 兼容 | 支持完整图谱及相关视图 |
| **Windsurf** | 兼容 | 原生支持 |
| **VSCodium / OpenVSCode** | 兼容 | 支持离线安装 |

## ⚠️ 使用提示

- 手动指定版本号必须以 `v` 开头（例如 `v19.0.1`）
- 若仓库已存在对应 tag，工作流会自动跳过避免重复打包
- 产物仅供个人测试与学习研究使用

## 🔗 相关链接

- [vscode-gitlens 官方仓库](https://github.com/gitkraken/vscode-gitlens)
- [GitLens 官网](https://www.gitkraken.com/gitlens)

## 📄 License

[MIT](./LICENSE)
