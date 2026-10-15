这是从「我的简历 App」导出的只读站点。

部署：把本目录所有文件上传到任意静态托管。
  · GitHub Pages：见 README-GitHub.md（3 分钟，含是否要 public 仓库的隐私提醒）
  · Netlify / Vercel：直接拖拽本目录即可
  · 对象存储 / 自己的服务器：上传后用域名访问 index.html

index.html 是只读页面，没有编辑器，访客无法修改你的内容。
og.png 是分享卡片图（微信 / LinkedIn / Twitter 抓取用），文件名不要改。
.nojekyll 与 404.html 是给 GitHub Pages 用的，其他托管可以忽略。

分享卡片（SEO）提示：
1. 在 App 的「后台 → SEO / 分享卡片」里填写部署后的站点地址，再重新导出，即可写入 canonical 与 og:url。
2. 微信、LinkedIn 等平台只抓静态 HTML 里的 meta 标签，所以请用这份导出的页面分享，而不是本地文件。
3. 若开启了密码访问，页面内容在 HTML 里是加密的，需要输入密码才能看到（对搜索引擎不可见）。

生成时间：2026-10-14T12:36:16.146Z