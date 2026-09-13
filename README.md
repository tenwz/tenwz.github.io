# 滕文证 Teng Wenzheng

极简个人博客：深色底、简洁字体、荧光绿链接，除一段基础样式表外没有多余样式。

部署在 GitHub Pages：<https://tenwz.github.io>

纯静态 HTML，无构建步骤、无依赖。

## 文件结构

```
├── index.html          # 主页（介绍 / 文章 / 项目 / 联系），兼作文章列表
├── posts/              # 文章目录，每篇文章一个 HTML 文件
│   └── px.html
└── css/style.css       # 基础样式（深底 + 系统字体 + 居中栏宽）
```

## 如何写新文章

1. 复制 `posts/px.html` 为新文件，改标题、日期和正文。
2. 在 `index.html` 的文章列表加一行 `<li>`（注意相对路径：文章内引用样式是 `../css/style.css`，主页里是 `css/style.css`）。
3. 提交推送，约一分钟后自动上线。

## 绑定自定义域名

如果想用自定义域名：

1. 在本仓库根目录添加 `CNAME` 文件，内容为域名；
2. 在域名 DNS 中添加 GitHub Pages 要求的记录；
3. 仓库 Settings → Pages 里确认域名并开启 HTTPS。
