# 滕文证 Teng Wenzheng

纯粹的 txt 网站：主页就是一个文件目录，点开即原始 `.txt` 纯文本。无样式、无构建、无依赖。

部署在 GitHub Pages：<https://tenwz.github.io>

## 文件结构

```
├── index.html   # 目录页，只有 txt 文件的链接（浏览器默认样式）
└── px.txt       # 文章，纯文本
```

## 如何写新文章

1. 在根目录新建一个 `.txt` 文件，直接写正文。
2. 在 `index.html` 的 `<pre>` 里加一行 `<a href="xxx.txt">xxx.txt</a>`。
3. 提交推送，约一分钟后自动上线。

## 绑定自定义域名

如果想用 `reed.wiki` 之类的自定义域名：

1. 在本仓库根目录添加 `CNAME` 文件，内容为域名（如 `reed.wiki`）；
2. 在域名 DNS 中添加 4 条 A 记录指向 `185.199.108.153` / `185.199.109.153` / `185.199.110.153` / `185.199.111.153`；
3. 仓库 Settings → Pages 里确认域名并开启 Enforce HTTPS。
