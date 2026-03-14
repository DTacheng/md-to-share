# MD to Share

将 Markdown 文件转换为单张原生长图，方便分享到微信、Discord 等平台。

## 安装

```bash
git clone https://github.com/DTacheng/md-to-share.git
cd md-to-share
npm install
```

## 使用方法

```bash
# 基本用法
node md2img.mjs <输入文件.md> <输出文件.png>

# 示例
node md2img.mjs "~/Downloads/文档.md" "~/Downloads/文档长图.png"
```

也可以添加全局命令：
```bash
npm link
md2img <输入文件.md> <输出文件.png>
```

## 特点

- **原生长图**：真正的文档流排版，不是 PPT 分页拼接
- **美观排版**：使用系统字体，支持表格、代码块、引用、列表等
- **微信友好**：宽度 800px，适合手机阅读
- **一步到位**：一条命令完成

## 样式说明

生成的长图样式：
- 标题：大字号，红色底部边框
- 二级标题：蓝色左侧边框
- 表格：斑马纹，悬停高亮
- 代码块：深色背景
- 行内代码：浅灰背景，红色文字
- 引用：蓝色边框 + 浅灰背景
- 分隔线：优雅分割

## 依赖

- Node.js 18+
- puppeteer-core
- marked
- Google Chrome（使用系统已安装的 Chrome）

## License

MIT
