# Markdown to Office Pasteboard

<p align="center">
  <img alt="GitHub Pages" src="https://img.shields.io/badge/GitHub%20Pages-ready-222222?style=for-the-badge&logo=githubpages&logoColor=white">
  <img alt="Static HTML" src="https://img.shields.io/badge/Static%20HTML-single%20file-E34F26?style=for-the-badge&logo=html5&logoColor=white">
  <img alt="No Build" src="https://img.shields.io/badge/No%20Build-zero%20setup-00A67E?style=for-the-badge&logo=githubactions&logoColor=white">
  <img alt="Office" src="https://img.shields.io/badge/Office-paste%20ready-2563EB?style=for-the-badge">
</p>

<p align="center">
  <img alt="Markdown" src="https://img.shields.io/badge/Markdown-GFM-111111?style=flat-square&logo=markdown&logoColor=white">
  <img alt="Tables" src="https://img.shields.io/badge/Tables-supported-146C5F?style=flat-square">
  <img alt="MathJax" src="https://img.shields.io/badge/Math-MathJax-1F4E79?style=flat-square&logo=mathworks&logoColor=white">
  <img alt="Marked" src="https://img.shields.io/badge/Parser-marked-2F855A?style=flat-square">
  <img alt="DOMPurify" src="https://img.shields.io/badge/Sanitize-DOMPurify-6B46C1?style=flat-square">
  <img alt="Clipboard" src="https://img.shields.io/badge/Clipboard-rich%20HTML-F59E0B?style=flat-square">
</p>

<p align="center">
  一个面向 Office 粘贴体验的 Markdown 富文本转换工具。
  <br>
  粘贴 Markdown 或拖入 `.md` 文件，点击按钮，把格式化后的 HTML 富文本复制到剪贴板。
</p>

---

## 功能亮点

- 支持直接粘贴 Markdown 文本。
- 支持拖拽 `.md`、`.markdown`、`.txt` 文件。
- 支持标题、段落、加粗、斜体、链接、引用、分隔线、列表、代码块。
- 支持 GFM 风格表格，并为 Office 粘贴做了边框和单元格样式。
- 支持行内公式 `$E=mc^2$` 和块级公式 `$$...$$`。
- 公式默认通过 MathJax 转为 Presentation MathML，优先让 Word、PowerPoint 识别为 Office 公式。
- 默认不复制纯文本，只把富文本作为主要粘贴内容；需要兜底文本时可手动勾选。

## 快速使用

直接打开：

```text
md2office.html
```

或者用本地静态服务器打开，剪贴板权限会更稳定：

```bash
python3 -m http.server 8765
```

然后访问：

```text
http://localhost:8765/md2office.html
```

使用流程：

1. 在左侧文本框粘贴 Markdown，或拖入 Markdown 文件。
2. 检查右侧预览。
3. 点击“复制到剪贴板”。
4. 切到 Office，直接粘贴。

## GitHub Pages 部署

仓库已经配好 GitHub Actions 工作流：

```text
.github/workflows/pages.yml
```

推荐部署方式：

1. 在 GitHub 创建一个新仓库。
2. 把本项目文件推送到默认分支，例如 `main`。
3. 打开仓库的 `Settings` -> `Pages`。
4. 在 `Build and deployment` 中选择 `GitHub Actions`。
5. 推送到 `main` 后，工作流会自动部署。

部署完成后，GitHub Actions 会在日志和部署摘要里显示站点 URL。

## 文件结构

```text
.
├── .github/
│   └── workflows/
│       └── pages.yml
├── .gitignore
├── .nojekyll
├── README.md
├── index.html
└── md2office.html
```

## 转换策略

这个工具复制的是 `text/html` 富文本剪贴板内容，并同时准备一个可选的 `text/plain` 兜底。

Office 对 HTML 粘贴的支持比对 Markdown 源码更好，所以工具会先把 Markdown 转成适合 Office 粘贴的 HTML：

- Markdown 解析：`marked`
- HTML 清理：`DOMPurify`
- 公式渲染：`MathJax`
- 粘贴格式：Clipboard API 的 `text/html`

## 已知限制

- 浏览器不能直接控制 Office 粘贴动作，所以需要手动切到 Office 后粘贴。
- 公式会以标准 Presentation MathML 写入 HTML 剪贴板；Office 是否转成原生可编辑公式仍取决于具体版本和粘贴通道。
- 如果网络无法访问 CDN，页面会退回到内置基础 Markdown 转换，复杂公式渲染会受影响。
- 不同版本的 Office 对 HTML、SVG、表格边框的粘贴支持可能略有差异。

## 开发

这是纯静态项目，不需要安装依赖。

本地预览：

```bash
python3 -m http.server 8765
```

静态检查可以用：

```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('md2office.html','utf8'); for (const m of html.matchAll(/<script(?![^>]*\\bsrc=)[^>]*>([\\s\\S]*?)<\\/script>/g)) new Function(m[1]); console.log('ok')"
```

## 致谢

- [marked](https://marked.js.org/)：Markdown 解析
- [DOMPurify](https://github.com/cure53/DOMPurify)：HTML 清理
- [MathJax](https://www.mathjax.org/)：公式渲染
- [GitHub Pages](https://pages.github.com/)：静态站点托管
