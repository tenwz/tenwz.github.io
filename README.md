# Tenwz 的博客

个人主页与博客，部署在 GitHub Pages：<https://tenwz.github.io>

纯静态 HTML + CSS，无构建步骤、无依赖。

## 文件结构

```
├── index.html          # 主页（自我介绍 / 业余项目 / 最新帖子 / 联系方式）
├── posts.html          # 全部帖子列表
├── about.html          # 关于页
├── posts/              # 文章目录，每篇文章一个 HTML 文件
│   └── hello-world.html
└── css/style.css       # 全站样式（含深色模式）
```

## 如何写新文章

1. 复制 `posts/hello-world.html` 为新文件，改标题、日期和正文。
2. 在 `index.html` 和 `posts.html` 的「帖子」区块各加一行链接（注意首页和 posts 页里的相对路径：文章内引用样式是 `../css/style.css`，列表页里是 `css/style.css`）。
3. 提交推送，约一分钟后自动上线。

## 如何改内容

- **名字 / 签名**：改各页面 `<section class="hero">` 里的文字。
- **项目**：改 `index.html` 中「业余项目」区块，一行一个项目。
- **联系方式**：改 `index.html` 中「保持联系」区块。
- **站点名 / logo**：改 `<title>` 和 `.site-mark` 里的 emoji。

## 深色模式

跟随系统自动切换，右上角按钮可手动切换并记住选择（存于 localStorage）。

## 绑定自定义域名

如果想用 `reed.wiki` 之类的自定义域名：

1. 在本仓库根目录添加 `CNAME` 文件，内容为域名（如 `reed.wiki`）；
2. 在域名 DNS 中添加 4 条 A 记录指向 `185.199.108.153` / `185.199.109.153` / `185.199.110.153` / `185.199.111.153`；
3. 仓库 Settings → Pages 里确认域名并开启 Enforce HTTPS。
