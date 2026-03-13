# SF_blog 工作流指南

## 项目概述

这是一个基于 **Hugo** 静态网站生成器的博客项目，使用 `hugo-theme-cleanwhite` 主题，通过 **GitHub Actions** 自动部署到 **GitHub Pages**。

- **仓库地址**: https://github.com/sha0fengGuo/SF_blog
- **网站地址**: https://sha0fengGuo.github.io/SF_blog/

---

## 个性化配置

编辑 `config.toml`，修改以下内容：
- `title` — 网站标题
- `SEOTitle` — 浏览器标签页显示的标题
- `description` — 网站描述
- `slogan` — 首页大图上的标语
- `header_image` — 首页背景大图（放在 `static/img/` 下）
- `sidebar_about_description` — 侧边栏简介
- `sidebar_avatar` — 侧边栏头像（放在 `static/img/` 下）
- `[params.social]` — 你的社交链接（GitHub、邮箱等）
- `[[params.additional_menus]]` — 导航栏菜单项

---

## 日常工作流

### 写新文章

```bash
# 1. 创建新文章（会自动生成模板）
hugo new post/my-new-post.md

# 2. 编辑文章
#    用 VS Code 打开 content/post/my-new-post.md 进行编辑
#    注意：文章头部的 draft: true 需要改为 draft: false 才会发布

# 3. 本地预览（可选）
hugo server -D
#    浏览器打开 http://localhost:1313/SF_blog/ 预览

# 4. 确认无误后，推送到 GitHub（自动部署）
git add .
git commit -m "新文章：我的新文章标题"
git push
```

### 文章格式说明（Front Matter）

每篇文章开头需要包含元信息：

```markdown
---
title: "文章标题"
date: 2026-03-13
draft: false
image: "img/post-bg-example.jpg"
categories:
  - notes
tags:
  - Hugo
  - tutorial
---

正文内容从这里开始...

<!--more-->

正文的其余部分（more 之前的内容会显示为摘要）
```

- `draft: true` → 草稿，不会发布
- `draft: false` → 正式发布
- `image` → 文章头图（可选，放在 `static/img/` 下）
- `<!--more-->` → 摘要分隔符，之前的内容显示在首页列表

---

## 常用命令速查

| 命令 | 说明 |
|------|------|
| `hugo new post/xxx.md` | 创建新文章 |
| `hugo server -D` | 本地预览（含草稿） |
| `hugo server` | 本地预览（不含草稿） |
| `hugo` | 生成静态文件到 `public/` |
| `git add .` | 暂存所有更改 |
| `git commit -m "说明"` | 提交更改 |
| `git push` | 推送到 GitHub（触发自动部署） |
| `git status` | 查看当前状态 |
| `git log --oneline -5` | 查看最近5条提交 |

---

## 部署流程（自动）

```
你写文章 → git push → GitHub Actions 自动运行 → 构建 Hugo → 部署到 gh-pages 分支 → 网站更新
```

你只需要 `git add` → `git commit` → `git push`，其余全部自动完成。

---

## 项目结构说明

```
SF_blog/
├── config.toml              # 主配置文件（网站设置、菜单、主题参数等）
├── content/                 # 文章内容
│   ├── post/                # 博客文章目录（cleanwhite 主题用 post/）
│   └── about.md             # 关于页面
├── static/                  # 静态资源
│   ├── img/                 # 图片（头图、头像、背景等）
│   └── images/              # 其他图片
├── themes/                  # 主题（Git 子模块）
│   └── hugo-theme-cleanwhite/
├── .github/workflows/
│   └── gh-pages.yml         # GitHub Actions 自动部署配置
├── .gitignore               # Git 忽略规则（public/ 等）
└── WORKFLOW.md              # 本文件
```

---

## 添加图片

1. 将图片放到 `static/img/` 目录下
2. 在文章中引用：
   ```markdown
   ![图片描述](/SF_blog/img/my-image.jpg)
   ```
3. 设置文章头图（在 front matter 中）：
   ```yaml
   image: "img/my-image.jpg"
   ```

---

## 常见问题

### Q: 推送后网站没更新？
1. 打开 https://github.com/sha0fengGuo/SF_blog/actions 查看 Actions 是否成功
2. 如果失败，点击进去查看错误日志

### Q: 本地预览正常但线上样式不对？
检查 `config.toml` 里的 `baseurl` 是否正确设置为 `https://sha0fengGuo.github.io/SF_blog/`

### Q: 文章写了但网站上看不到？
检查文章头部的 `draft` 是否设为 `false`

### Q: 如何绑定自定义域名？
1. 在 GitHub 仓库 Settings → Pages → Custom domain 填入你的域名
2. 在域名 DNS 中添加 CNAME 记录指向 `sha0fengGuo.github.io`
3. 修改 `config.toml` 中的 `baseurl` 为你的域名


你的文章日期是 2026-03-15，但今天是 3月13日。Hugo 默认不显示未来日期的文章。

两个解决办法，任选一个：

方法1：把日期改为今天或之前


date: 2026-03-13
方法2：启动时加参数显示未来文章


hugo server -D -F
-D = 显示草稿，-F = 显示未来日期的文章

建议用方法1，把日期改成实际写作日期就