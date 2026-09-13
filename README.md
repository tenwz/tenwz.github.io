# 滕文证 Teng Wenzheng

个人静态博客，记录软件工程、Agent runtime 和本地优先工具方面的想法与实验。

网站部署在 GitHub Pages：<https://tenwz.github.io>

项目保持纯静态 HTML，无构建步骤、无运行时依赖。页面使用一套统一的响应式设计系统：浅色背景、深色正文、蓝色强调色和适合中英混排的系统字体。

## 文件结构

```
├── index.html          # 主页：个人介绍、文章和项目
├── posts/              # 文章目录，每篇文章一个 HTML 文件
│   └── px.html
└── css/style.css       # 首页与文章页共用的响应式样式
```

## 如何写新文章

1. 复制 `posts/px.html` 为新文件，更新标题、日期和正文。
2. 在 `index.html` 的文章区域添加一张 featured card。
3. 文章页使用 `../css/style.css`，主页使用 `css/style.css`。
4. 提交并推送到 `main`，GitHub Pages 会自动部署。

## 绑定自定义域名

如果想使用自定义域名：

1. 在仓库根目录添加 `CNAME` 文件，内容为域名；
2. 在域名 DNS 中添加 GitHub Pages 要求的记录；
3. 在仓库 Settings → Pages 中确认域名并开启 HTTPS。
