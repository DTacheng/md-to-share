---
name: md-to-share
description: "将 Markdown 文件转换为原生长图，方便分享到微信/Discord 等平台。生成单张完整长图，排版美观。"
---

# MD 转长图 (MD to Share)

将 Markdown 文件转换为单张原生长图，方便分享到微信、Discord 等平台。

## 快速使用

当用户要求"转发"、"转成图片"、"方便分享"、"生成长图"时使用此 skill。

## 使用方法

### 1. 转换命令

```bash
md2img <输入文件.md> <输出文件.png>
```

示例：
```bash
md2img "~/Downloads/文档.md" "~/Downloads/文档长图.png"
```

### 2. 发送到 Discord

使用 message 工具的 media 参数发送图片：
```json
{
  "action": "send",
  "target": "频道ID",
  "message": "简短说明",
  "media": "/完整/图片/路径.png"
}
```

### 3. 发送到微信/其他平台

- 图片生成在指定路径，直接在文件管理器中获取
- 或复制到剪贴板后粘贴发送

## 工具说明

`md2img` 是一个基于 Puppeteer 的 Markdown 转长图工具，特点：

- **原生长图**：真正的文档流排版，不是 PPT 分页拼接
- **美观排版**：使用系统字体，支持表格、代码块、引用、列表等
- **微信友好**：宽度 800px，适合手机阅读
- **一步到位**：一条命令完成

## 依赖

- Node.js 18+
- puppeteer-core
- marked
- Google Chrome（使用系统已安装的 Chrome）

依赖已安装在 `~/.openclaw/skills/md-to-share/node_modules/`

## 样式说明

生成的长图样式：
- 标题：大字号，红色底部边框
- 二级标题：蓝色左侧边框
- 表格：斑马纹，悬停高亮
- 代码块：深色背景
- 行内代码：浅灰背景，红色文字
- 引用：蓝色边框 + 浅灰背景
- 分隔线：优雅分割

## 示例

用户说"把这个发到微信"或"方便转发"时：
1. 读取 MD 文件内容
2. 执行 `md2img` 转换为长图
3. 将图片路径告知用户，或直接发送到指定平台
