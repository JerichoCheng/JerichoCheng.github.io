# 技术博客脚手架（Hugo + PaperMod）

这是一个可以直接使用的 Hugo 博客项目骨架，主题用的是 [PaperMod](https://github.com/adityatelange/hugo-PaperMod)。

## 目录说明

- `hugo.yaml` — 站点主配置（标题、菜单、PaperMod 各项参数）
- `content/posts/hello-world.md` — 示例文章，演示代码高亮和 KaTeX 数学公式
- `content/about.md` / `archives.md` / `search.md` — 关于页、归档页、搜索页
- `layouts/partials/extend_head.html` — 数学公式（KaTeX）支持，文章 front matter 加 `math: true` 才会加载
- `layouts/partials/comments.html` — giscus 评论区，需要替换成你自己的仓库信息
- `themes/PaperMod/` — 主题源码（当前是直接拷贝进来的普通目录，方便你解压即用）
- `.github/workflows/hugo.yml` — GitHub Actions 自动构建部署工作流

## 开始之前你需要改的地方

1. `hugo.yaml` 里所有 `yourusername` / `你的名字` 替换成你自己的信息，`baseURL` 改成你实际的域名（默认是 `https://<你的用户名>.github.io/`）。
2. `layouts/partials/comments.html` 里的 `data-repo` / `data-repo-id` / `data-category-id`：去 [giscus.app](https://giscus.app) 用你自己的仓库生成，记得先在仓库 Settings → General → Features 里打开 Discussions。
3. （可选）`static/` 目录下放你自己的 `favicon.ico` 等图标文件。
4. （可选，推荐）如果你想让主题以后能用 `git submodule update --remote` 一键跟着上游更新，把 `themes/PaperMod` 换成正式的 submodule：
   ```bash
   rm -rf themes/PaperMod
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   ```
   不介意手动更新的话，保持现在这样直接拷贝进来也完全没问题。

## 本地预览

```bash
hugo server -D
```

打开 http://localhost:1313 查看效果。

## 发布到 GitHub Pages

1. 在 GitHub 新建一个仓库（建议直接叫 `你的用户名.github.io`，这样默认就是根域名）。
2. 把这个文件夹的内容推上去：
   ```bash
   git init
   git add .
   git commit -m "init blog"
   git branch -M main
   git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
   git push -u origin main
   ```
3. 仓库页面 → Settings → Pages → Build and deployment → Source 选择 **GitHub Actions**。
4. 推送后 Actions 会自动跑 `.github/workflows/hugo.yml`，跑完就能在 `https://你的用户名.github.io` 看到博客。

## 写新文章（中英双语）

项目已经配好了中英双语结构（默认语言是中文，挂在根域名下；英文版在 `/en/` 路径下），文章按文件名后缀区分语言：

```bash
hugo new content posts/我的新文章.zh.md   # 中文版
hugo new content posts/我的新文章.en.md   # 英文版（可选，不是每篇都要写）
```

两个文件的文件名前半部分（`我的新文章`）要完全一样，PaperMod 才会认出它们是同一篇文章的两个语言版本，在文章页里显示语言切换按钮。只想写一种语言就只建一个文件，不用两个都写。

写完把 front matter 里的 `draft: true` 改成 `false`（或删掉这一行），`git push` 就会自动发布。

关于/归档/搜索这几个功能页同理，也是按 `xxx.zh.md` / `xxx.en.md` 的方式各自维护一份，已经放在 `content/` 目录下做了示例，直接改里面的文字就行。

本地已经用 Hugo v0.166.0（extended 版）验证过可以正常构建，PaperMod 要求 Hugo ≥ v0.146.0，注意你本地/CI 的版本不要太旧。
