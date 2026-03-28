# MD to Share

[中文](#中文) | [English](#english)

---

## 中文

将 Markdown 文件转换为高质量长图，可被 OpenClaw、Claude Code 等 AI Agent 直接调用。支持分享到微信、Discord、iMessage 等平台。

### v2.0 升级亮点

> **从 Puppeteer 迁移到 Playwright** — 自带 Chromium，不再依赖系统浏览器，彻底解决多 Agent 并行时的浏览器冲突问题。

| 项目 | v1 (旧版) | v2 (当前) |
|------|-----------|-----------|
| 浏览器引擎 | puppeteer-core + 系统 Chrome | **Playwright + 内置 Chromium** |
| 系统依赖 | 必须安装 Chrome/Chromium | **无需任何系统浏览器** |
| 多 Agent 并行 | 与 Chrome MCP 冲突 | **完全隔离，互不干扰** |
| 渠道适配 | 一刀切 | **`--channel` 按渠道智能切图** |
| 稳定性 | 浏览器启动偶发失败 | **内置重试机制** |
| 安装方式 | npm install + 手动安装 Chrome | **npm install 一步到位** |

### 安装

```bash
git clone https://github.com/DTacheng/md-to-share.git
cd md-to-share
npm install   # 自动下载 Playwright 内置 Chromium，无需其他操作
```

也支持 `bun install`。

### 使用方法

```bash
# 基本用法
md2img report.md output.jpg

# 发到 Discord（自动切图，确保清晰可读）
md2img report.md output.jpg --channel=discord

# 发到微信（保留原始长图，微信原生支持）
md2img report.md output.jpg --channel=wechat

# 本地使用（完整长图，无切分）
md2img report.md output.jpg --channel=local

# 自定义主题和超时
md2img report.md output.jpg --theme=dark --timeout=60000
```

### 渠道智能切图

不同平台对图片有不同限制。`--channel` 参数让工具自动适配：

| 渠道 | 行为 | 原因 |
|------|------|------|
| `discord` | 自动切图（每张 ≤1800px 高） | Discord 会将超高图缩放到无法阅读 |
| `wechat` | 保留完整长图 | 微信原生支持长图浏览 |
| `imessage` | 保留完整长图 | iMessage 支持长图 |
| `local` | 保留完整长图 | 本地无平台限制 |
| 未指定 | 保留完整长图 | 安全默认值 |

切图在语义边界（h1/h2/hr）处进行，不会在段落中间断开。

### AI Agent 使用指引

如果你是 AI Agent（OpenClaw、Claude Code 等），在调用此工具前：

1. **确定目标渠道** — 用户要发到哪个平台？
2. **如果确定** — 传入对应的 `--channel` 参数
3. **如果不确定** — 先询问用户："您想把图片发到哪个平台？（Discord / 微信 / iMessage / 本地保存）"

```bash
# OpenClaw agent 发送到 Discord
md2img report.md report.jpg --preset=openclaw --channel=discord

# OpenClaw agent 发送到微信
md2img report.md report.jpg --preset=openclaw --channel=wechat
```

### 完整参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--preset=<name>` | 配置预设：`openclaw` \| `generic` | 自动检测 |
| `--channel=<name>` | 目标渠道：`discord` \| `wechat` \| `imessage` \| `local` | 无 |
| `--width=<px>` | CSS 宽度（像素） | 预设值 |
| `--scale=<factor>` | 设备缩放因子 | 2 |
| `--max-size=<MB>` | 分割文件大小阈值 | 预设值 |
| `--max-height=<px>` | 每张图最大实际像素高度 | 渠道值 |
| `--quality=<1-100>` | JPEG 质量 | 85 |
| `--theme=<light\|dark>` | 强制主题 | 按时间自动 |
| `--timeout=<ms>` | 浏览器超时（毫秒） | 30000 |

### 配置预设

| 预设 | CSS 宽度 | 缩放 | 实际宽度 | 文件上限 | 场景 |
|------|----------|------|----------|----------|------|
| `openclaw` | 600px | 2x | 1200px | 5MB | OpenClaw agent |
| `generic` | 800px | 2x | 1600px | 8MB | Claude Code / 本地 |

### 输出格式

- `.jpg` / `.jpeg` — JPEG 格式（默认，文件更小）
- `.png` — PNG 格式（无损，文件更大）

### 退出码

| 代码 | 描述 |
|------|------|
| 0 | 成功 |
| 1 | 参数无效 |
| 2 | 文件未找到 |
| 3 | 文件读取错误 |
| 4 | 浏览器未安装（运行 `npx playwright install chromium`） |
| 5 | 浏览器启动错误 |
| 6 | 渲染错误 |
| 7 | 截图错误 |
| 8 | 输出写入错误 |

### 环境变量

| 变量 | 描述 |
|------|------|
| `CHROME_PATH` | 覆盖 Playwright 内置 Chromium 路径 |
| `MD2IMG_TIMEOUT` | 覆盖默认超时（毫秒） |

### 依赖

- Node.js 18+
- playwright（自带 Chromium，无需系统浏览器）
- marked（Markdown 解析器）

### 从 v1 升级

```bash
cd md-to-share
npm install   # 自动替换 puppeteer-core → playwright 并下载 Chromium
```

无需任何代码改动。如果之前有 `CHROME_PATH` 环境变量，可以删除（不再需要）。已有的调用方式完全兼容，新增的 `--channel` 和 `--timeout` 为可选参数。

---

## English

Convert Markdown files to high-quality long images, callable by AI Agents like OpenClaw and Claude Code. Supports sharing to WeChat, Discord, iMessage, and more.

### v2.0 Highlights

> **Migrated from Puppeteer to Playwright** — ships with bundled Chromium, no system browser dependency. Eliminates browser conflicts when multiple agents run in parallel.

| Feature | v1 (Old) | v2 (Current) |
|---------|----------|--------------|
| Browser Engine | puppeteer-core + system Chrome | **Playwright + bundled Chromium** |
| System Dependencies | Chrome/Chromium must be installed | **None** |
| Multi-Agent Parallel | Conflicts with Chrome MCP | **Fully isolated** |
| Channel Adaptation | One-size-fits-all | **`--channel` smart splitting** |
| Reliability | Occasional browser launch failures | **Built-in retry mechanism** |
| Installation | npm install + manual Chrome setup | **Single `npm install`** |

### Installation

```bash
git clone https://github.com/DTacheng/md-to-share.git
cd md-to-share
npm install   # Automatically downloads Playwright's bundled Chromium
```

Also works with `bun install`.

### Usage

```bash
# Basic usage
md2img report.md output.jpg

# For Discord (auto-split for readability)
md2img report.md output.jpg --channel=discord

# For WeChat (keep original long image)
md2img report.md output.jpg --channel=wechat

# For local use (full long image, no splitting)
md2img report.md output.jpg --channel=local

# Custom theme and timeout
md2img report.md output.jpg --theme=dark --timeout=60000
```

### Channel-Aware Smart Splitting

Different platforms handle images differently. The `--channel` flag auto-adapts:

| Channel | Behavior | Reason |
|---------|----------|--------|
| `discord` | Auto-split (max 1800px height per image) | Discord scales tall images to unreadable sizes |
| `wechat` | Keep full long image | WeChat natively supports long image viewing |
| `imessage` | Keep full long image | iMessage handles long images well |
| `local` | Keep full long image | No platform constraints |
| Not specified | Keep full long image | Safe default |

Splitting occurs at semantic boundaries (h1/h2/hr), never mid-paragraph.

### AI Agent Usage Guide

If you are an AI Agent (OpenClaw, Claude Code, etc.), before calling this tool:

1. **Determine the target channel** — Where does the user want to send the image?
2. **If known** — Pass the corresponding `--channel` parameter
3. **If unknown** — Ask the user first: "Which platform will you share this on? (Discord / WeChat / iMessage / Local)"

```bash
# OpenClaw agent → Discord
md2img report.md report.jpg --preset=openclaw --channel=discord

# OpenClaw agent → WeChat
md2img report.md report.jpg --preset=openclaw --channel=wechat
```

### Full Options

| Option | Description | Default |
|--------|-------------|---------|
| `--preset=<name>` | Config preset: `openclaw` \| `generic` | Auto-detect |
| `--channel=<name>` | Target: `discord` \| `wechat` \| `imessage` \| `local` | None |
| `--width=<px>` | CSS width in pixels | Preset value |
| `--scale=<factor>` | Device scale factor | 2 |
| `--max-size=<MB>` | File size split threshold | Preset value |
| `--max-height=<px>` | Max actual pixel height per image | Channel value |
| `--quality=<1-100>` | JPEG quality | 85 |
| `--theme=<light\|dark>` | Force theme | Auto by time |
| `--timeout=<ms>` | Browser timeout in ms | 30000 |

### Configuration Presets

| Preset | CSS Width | Scale | Actual Width | Max Size | Use Case |
|--------|-----------|-------|--------------|----------|----------|
| `openclaw` | 600px | 2x | 1200px | 5MB | OpenClaw agents |
| `generic` | 800px | 2x | 1600px | 8MB | Claude Code / local |

### Output Formats

- `.jpg` / `.jpeg` — JPEG format (default, smaller files)
- `.png` — PNG format (lossless, larger files)

### Exit Codes

| Code | Description |
|------|-------------|
| 0 | Success |
| 1 | Invalid arguments |
| 2 | File not found |
| 3 | File read error |
| 4 | Browser not installed (run `npx playwright install chromium`) |
| 5 | Browser launch error |
| 6 | Render error |
| 7 | Screenshot error |
| 8 | Output write error |

### Environment Variables

| Variable | Description |
|----------|-------------|
| `CHROME_PATH` | Override Playwright's bundled Chromium |
| `MD2IMG_TIMEOUT` | Override default timeout in ms |

### Dependencies

- Node.js 18+
- playwright (bundles its own Chromium — no system browser required)
- marked (Markdown parser)

### Upgrading from v1

```bash
cd md-to-share
npm install   # Automatically replaces puppeteer-core with playwright + Chromium
```

No code changes needed. You can remove the `CHROME_PATH` environment variable if previously set (no longer required). All existing CLI arguments remain compatible. The new `--channel` and `--timeout` flags are optional additions.

---

## License

MIT
