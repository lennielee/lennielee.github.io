# 网站更新指南

网站地址：https://lennielee.github.io
本地目录：`~/personal/lennielee.github.io`

**所有内容都在 `content/` 文件夹里，平时只需要改这里，不用动代码。**

| 想改什么 | 改哪个文件 |
|---|---|
| 简介（首页 About） | `content/bio.md` |
| 研究方向（头像下方） | `content/about.toml` 里的 `research_interests` |
| 新闻（首页 News） | `content/news.toml` |
| 论文（Publications 页 + 首页 Selected Publications） | `content/publications.bib` |
| 奖项 | `content/awards.toml` |
| 教学 | `content/teaching.toml` |
| 学术服务 | `content/services.toml` |
| CV | `content/cv.md` |
| 名字、头衔、单位、邮箱、GitHub、导航栏 | `content/config.toml` |
| 头像 | 替换 `public/photo.jpg`（建议正方形，约 800×800） |

---

## 发布：改完之后怎么上线

```bash
cd ~/personal/lennielee.github.io
git add -A
git commit -m "简单写一下改了什么"
git push
```

push 之后 1–3 分钟网站自动更新。
部署进度可以在 https://github.com/lennielee/lennielee.github.io/actions 查看：绿色对勾表示成功，红叉表示失败（见文末「出问题了怎么办」）。

> 顺手把 `config.toml` 里的 `last_updated` 改成当天日期，它会显示在页面底部。

---

## 加一条新闻

打开 `content/news.toml`，在**最上面**加一段（新的放前面）：

```toml
[[news]]
date = "2027-03"
content = "Our paper on lossy compression was accepted to **SC '27**! 🎉"

[[news]]
date = "2026-09"
content = "Started Ph.D. in Computer Science at Oregon State University."
```

- `date` 随便写，比如 `"2027-03"`、`"Mar 2027"` 都可以，原样显示。
- `content` 支持 Markdown：`**加粗**`，`[链接文字](https://...)`。
- 一条新闻都没有的时候，首页的 News 区块会自动隐藏。

---

## 加一篇论文

论文统一写在 `content/publications.bib`，格式是标准 BibTeX。最省事的做法是从 Google Scholar 或 DBLP 复制 BibTeX，粘贴进来，再按需加几个 PRISM 专用字段：

```bibtex
@inproceedings{li2027compress,
  title     = {Fast Error-Bounded Lossy Compression on GPUs},
  author    = {Linze Li* and Jane Doe and John Smith},
  booktitle = {Proceedings of SC '27},
  year      = {2027},
  doi       = {10.1145/xxxxxxx},

  selected    = {true},
  preview     = {li2027compress.png},
  description = {One-sentence summary shown under the title.},
  code        = {https://github.com/lennielee/xxx},
  keywords    = {Lossy Compression, GPU, HPC}
}
```

| 字段 | 作用 |
|---|---|
| `selected = {true}` | **会出现在首页的 Selected Publications 里**（最多显示 5 篇，数量在 `about.toml` 的 `limit` 改）。不写这个字段的论文只出现在 Publications 页 |
| `preview` | 论文配图。把图片放到 `public/papers/` 文件夹（没有就新建），这里只写文件名 |
| `description` | 标题下面的一句话简介 |
| `code` | 代码链接，会显示一个 Code 按钮 |
| `doi` / `url` | 论文链接 |
| `keywords` | 关键词标签，逗号分隔 |

**作者标记**（写在 `author` 字段里的名字后面）：
- `*` 表示通讯作者，例如 `Linze Li*`
- `#` 表示共同一作，例如 `Linze Li# and Jane Doe#`
- 你自己的名字 `Linze Li` 会自动加粗，不用另外处理。

期刊论文用 `@article{...}`，会议名字段 `booktitle` 改成期刊名字段 `journal`。

论文都没有 `selected` 的时候，首页的 Selected Publications 区块会自动隐藏。

---

## 加奖项 / 教学 / 服务

这三个文件格式一样，每项一个 `[[items]]` 块：

```toml
[[items]]
title = "Best Paper Award"
subtitle = "SC '27"                  # 可选：机构 / 会议 / 课程角色
date = "2027"
content = "Awarded for ..."          # 可选，支持 Markdown
tags = ["Conference"]                # 可选
```

> `teaching.toml` 和 `services.toml` 现在有一行 `items = []`，表示暂时为空。
> **加第一项的时候要把这一行删掉**，否则构建会报错。

---

## 改 CV

`content/cv.md` 就是普通 Markdown，直接改即可：

```markdown
## Education

**Oregon State University**, Ph.D. in Computer Science, *2026 - Present*

## Publications

- L. Li, ... *Fast Error-Bounded Lossy Compression on GPUs*. SC '27.
```

---

## 加一个新页面（比如 Projects）

1. 新建 `content/projects.toml`：
   ```toml
   type = "card"            # card = 卡片列表，text = Markdown 页面
   title = "Projects"
   description = "Things I've built."

   [[items]]
   title = "Project Name"
   content = "What it does. [GitHub](https://github.com/lennielee/xxx)"
   ```
2. 在 `content/config.toml` 末尾加一个导航项，它会按顺序出现在导航栏：
   ```toml
   [[navigation]]
   title = "Projects"
   type = "page"
   target = "projects"
   href = "/projects"
   ```

想去掉某个导航项，就删掉对应的 `[[navigation]]` 块。

---

## 本地预览（可选）

不预览也可以，直接 push 上网站看效果。想先在本地看的话：

1. 安装 Node.js 22 或更高版本：https://nodejs.org/en/download
2. 运行：
   ```bash
   cd ~/personal/lennielee.github.io
   npm install        # 只需第一次运行
   npm run dev
   ```
3. 浏览器打开 http://localhost:3000 ，改文件后页面会自动刷新。按 `Ctrl+C` 退出。

---

## 出问题了怎么办

- **网站没变化**：先等 2–3 分钟，再强制刷新浏览器（`Ctrl+Shift+R`，Mac 上是 `Cmd+Shift+R`）。
- **Actions 页面出现红叉**：点进去看 "Build site" 这一步的报错，通常是格式写错了：
  - `.toml`：引号没闭合，或者 `items = []` 和 `[[items]]` 同时存在。
  - `.bib`：括号 `{}` 不配对，或者两个条目之间漏了逗号或 `}`。
  - 修好后重新 commit + push 即可。网站会保持上一次成功的版本，不会挂掉。
- **push 被拒（rejected）**：说明 GitHub 上有你在别处做的改动，先运行 `git pull --rebase` 再 `git push`。
