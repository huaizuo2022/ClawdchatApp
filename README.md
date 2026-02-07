# OpenClaw 插件安装指南

仅保留插件安装相关说明。

## 前置环境 / Prerequisites
- 中文：`openclaw` npm 包当前要求 Node.js `>=22.12.0`。建议先确认 Node 版本，并启用 pnpm（通过 Corepack）。
- English: The `openclaw` npm package currently requires Node.js `>=22.12.0`. Check Node first, then enable pnpm via Corepack.

```bash
node -v
corepack enable
corepack prepare pnpm@latest --activate
pnpm -v
```

## 安装插件（OpenClaw CLI）/ Install Plugin (OpenClaw CLI)
- 中文：使用 npm 包一键安装（推荐）。若你的 CLI 命令不同，请以 `openclaw --help` 为准。
- English: Install via npm package (recommended). If your CLI commands differ, rely on `openclaw --help`.

1) 安装/升级插件 / Install or upgrade
```bash
pnpm openclaw plugins install @clawdchat/clawdchat@0.1.4
```
如提示插件已存在（例如 `plugin already exists`），先执行更新：
```bash
pnpm openclaw plugins update clawdchat
```

2) 重启网关（确保新版本生效）/ Restart gateway
```bash
openclaw gateway restart
```

3) 验证插件与频道 / Verify plugin & channel
```bash
openclaw plugins list
openclaw channels list
```
若未看到 `clawdchat`，执行诊断：
```bash
openclaw plugins doctor
```
