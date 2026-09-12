# tenwz 的博客

极简打印机 / 终端风格的个人主页与博客，部署在 GitHub Pages：<https://tenwz.github.io>

纯静态 HTML + CSS，无构建步骤、无依赖、无 JavaScript。

## 文件结构

```
├── index.html          # 主页（say hi / 项目 / 帖子 / 联系）
├── posts.html          # 全部帖子列表
├── about.html          # 关于页
├── posts/              # 文章目录，每篇文章一个 HTML 文件
│   └── hello-world.html
└── css/style.css       # 全站样式（等宽字体 + 穿孔纸边线）
```

## 设计说明

- 等宽打字机字体（Courier New / monospace），纯白纸面 + 黑墨单色
- 页面两侧的虚线是连续打印纸的穿孔边
- 分隔线全部是 `--` / 粗实线 / 虚线，页脚以 `- EOF -` 结束
- 左上角 `tenwz_` 带一个闪烁光标（唯一的 JavaScript 都没有，纯 CSS 动画）
- 链接悬停时黑底白字反色，像打印头扫过

## 如何写新文章

1. 复制 `posts/hello-world.html` 为新文件，改标题、日期和正文。
2. 在 `index.html` 和 `posts.html` 的帖子列表各加一行链接（注意相对路径：文章内引用样式是 `../css/style.css`，列表页里是 `css/style.css`）。
3. 提交推送，约一分钟后自动上线。

## 绑定自定义域名

如果想用 `reed.wiki` 之类的自定义域名：

1. 在本仓库根目录添加 `CNAME` 文件，内容为域名（如 `reed.wiki`）；
2. 在域名 DNS 中添加 4 条 A 记录指向 `185.199.108.153` / `185.199.109.153` / `185.199.110.153` / `185.199.111.153`；
3. 仓库 Settings → Pages 里确认域名并开启 Enforce HTTPS。
