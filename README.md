# clawdchat

中文：Clawdchat 让用户使用自有账号体系登录，并在应用内与 Clawdbot 对话。配对与诊断通过 Clawdbot Gateway + OpenClaw 频道插件完成。

English: Clawdchat lets users sign in with a first-party account system and chat with Clawdbot inside the app. Pairing and diagnostics run via Clawdbot Gateway + the OpenClaw channel plugin.

## 目录结构 / Repository Layout
- backend/    FastAPI 服务 / FastAPI service
- frontend/   Flutter 客户端 / Flutter app
- plugins/    OpenClaw 频道插件 / OpenClaw channel plugin
- docs/       本地运行与说明 / Setup notes
- specs/      产品规格与契约 / Specs & API contracts

## 快速开始（本地）/ Quickstart (Local)

### 后端（FastAPI）/ Backend (FastAPI)
```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```
中文：生产默认地址 `https://api-love2.huaizuo2029.cn`，本地开发请用 `http://127.0.0.1:8000`。
English: Production default API base is `https://api-love2.huaizuo2029.cn`. For local dev, use `http://127.0.0.1:8000`.

### 前端（Flutter）/ Frontend (Flutter)
```bash
cd frontend
flutter pub get
flutter run --dart-define=API_BASE_URL=http://127.0.0.1:8000
```

## Clawdbot Gateway + OpenClaw 频道 / Channel

### 前置环境 / Prerequisites
中文：`openclaw` npm 包当前要求 Node.js `>=22.12.0`。建议先确认 Node 版本，并启用 pnpm（通过 Corepack）。

English: The `openclaw` npm package currently requires Node.js `>=22.12.0`. Check Node first, then enable pnpm via Corepack.

```bash
node -v
corepack enable
corepack prepare pnpm@latest --activate
pnpm -v
```

### 安装插件（OpenClaw CLI）/ Install Plugin (OpenClaw CLI)
中文：使用 npm 包一键安装（推荐）。若你的 CLI 命令不同，请以 `openclaw --help` 为准。

English: Install via npm package (recommended). If your CLI commands differ, rely on `openclaw --help`.

1) 安装/升级插件 / Install or upgrade
```bash
pnpm openclaw plugins install @clawdchat/clawdchat@0.1.4
```
如提示插件已存在（例如 `plugin already exists`），先执行更新：
```bash
pnpm openclaw plugins update clawdchat
```

2) 重启网关（确保新版本生效） / Restart gateway
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

### 登录并获取配对码 / Login and Get Pairing Code
```bash
openclaw channels login --channel clawdchat
```
终端会输出 6 位配对码（有效期 10 分钟）。

### App 内完成配对 / Complete Pairing in App
在 Clawdchat App：Settings -> Clawdbot 配对，输入配对码确认。若提示未连接，请重新生成新配对码后再试。

## 配置 / Configuration

### 后端 .env / Backend .env
```env
DATABASE_URL=sqlite:///./app.db
JWT_SECRET=change-me
JWT_ALGORITHM=HS256
JWT_EXPIRES_MINUTES=1440
CHANNEL_PAIRING_TTL_MINUTES=10
CHANNEL_REPLY_TIMEOUT_SECONDS=20
CHANNEL_SHARED_KEY=
EMAIL_OTP_TTL_MINUTES=10
EMAIL_OTP_MAX_ATTEMPTS=5
EMAIL_OTP_DEBUG=false
SMTP_HOST=
SMTP_PORT=587
SMTP_USER=
SMTP_PASSWORD=
SMTP_FROM=
SMTP_USE_TLS=true
SMTP_USE_SSL=false
APNS_KEY_PATH=/abs/path/AuthKey_XXXX.p8
APNS_KEY_B64=            # 可选：将 .p8 base64 后直接填，优先级高于 PATH
APNS_KEY_ID=XXXXXX
APNS_TEAM_ID=HBAWS8SSHF
APNS_BUNDLE_ID=com.clawdbot.chat.frontend
APNS_USE_SANDBOX=true
```

### 插件环境变量（可选）/ Plugin env (optional)
```env
CLAWDCHAT_API_BASE_URL=https://api-love2.huaizuo2029.cn
CLAWDCHAT_SHARED_KEY=
```

### 前端运行参数（可选）/ Frontend runtime defines (optional)
```bash
flutter run \
  --dart-define=API_BASE_URL=http://127.0.0.1:8000 \
  --dart-define=SKIP_AUTH=true \
  --dart-define=DEV_EMAIL=dev@clawdbot.app \
  --dart-define=DEV_PASSWORD=password123 \
  --dart-define=PRIVACY_POLICY_URL=https://clawdbot.app/privacy \
  --dart-define=TERMS_URL=https://clawdbot.app/terms
```
默认 `SKIP_AUTH=false`，生产环境需要正常登录。

## 备注 / Notes
- 中文：不要提交真实密钥，使用 `.env` 本地配置。
- English: Do not commit real secrets; keep them in `.env` locally.

## License
Apache-2.0
