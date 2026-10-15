# 部署到 GitHub Pages（3 分钟）

这个目录就是一份可以直接发布的静态站点：不需要构建、不需要后端，所有路由都是 # 锚点，所以 GitHub Pages 不需要任何重写规则。

## 一、新建仓库并上传

网页方式（最省事）：
1. 打开 https://github.com/new ，仓库名例如 `resume`，选择 Public，不要勾选任何初始化文件。
2. 进入仓库 → `Add file` → `Upload files` → 把本目录的**所有文件**拖进去（包括 .nojekyll）。
3. 提交信息写 `deploy resume`，点 `Commit changes`。

命令行方式：
```bash
cd 这个目录
git init -b main
git add -A
git commit -m "deploy resume"
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

## 二、开启 Pages

1. 仓库 → Settings → Pages。
2. Source 选 `Deploy from a branch`，Branch 选 `main` + `/ (root)`，点 Save。
3. 等 1–2 分钟，页面顶部会出现网址：https://<你的用户名>.github.io/<仓库名>/

## 三、回填站点地址（让分享卡片生效）

1. 回到 App → 后台 → SEO / 分享 → 把上面的网址填进「部署后的站点地址」，保存。
2. 重新导出一次只读站点，用新文件覆盖仓库里的 index.html（这次导出的 HTML 会带上 canonical 与 og:url）。
3. 把这个网址粘贴到微信 / LinkedIn / Twitter，就会显示标题、简介与分享图。

## 四、以后更新内容

App 里改完内容 → 后台 → 发布 → 重新「导出只读站点」→ 把新的 index.html（和 og.png）覆盖上传即可。

## 五、可选：自定义域名

1. 在域名 DNS 里加一条 CNAME 记录指向 `<你的用户名>.github.io`。
2. 仓库 → Settings → Pages → Custom domain 填你的域名，勾选 Enforce HTTPS。
3. 仓库根目录会生成 `CNAME` 文件（也可以自己上传一个）。

## 六、可选：用 GitHub Actions 自动部署

如果你想「推上去就自动发布」（而不是用分支方式），把 Pages 的 Source 改成 `GitHub Actions`，
然后在仓库里新建 `.github/workflows/pages.yml`：

```yaml
name: Deploy resume site
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: true
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: .
      - id: deployment
        uses: actions/deploy-pages@v4
```

## 七、注意事项

- **隐私**：GitHub 免费账号即使仓库设为 Private，Pages 站点也是公开可访问的。不要把包含私密信息（身份证号、住址、电话）的内容推上来；需要保护时请使用「导出为密码访问页面」（正文 AES-GCM 加密，没有密码看不到内容）。
- **文件名大小写**：GitHub Pages 的服务器区分大小写，请保持导出的文件名原样（index.html / og.png / favicon.svg）。
- **缓存**：微信 / LinkedIn 会缓存分享卡片，改了标题或图之后可能需要换个链接或等一段时间才会刷新。
- **仓库名**：仓库名会成为网址的一部分，建议用简单的英文名（如 `resume`）。
- **.nojekyll**：这个空文件告诉 GitHub Pages 不要用 Jekyll 处理，避免以下划线开头的文件被忽略。

生成时间：2026-10-15T01:05:18.640Z