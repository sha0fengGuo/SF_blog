# SF_blog 工作流指南

## 项目概述

这是一个基于 **Hugo** 静态网站生成器的博客项目，使用 `hugo-theme-den` 主题，通过 **GitHub Actions** 自动部署到 **GitHub Pages**。

- **仓库地址**: https://github.com/battle1king/SF_blog
- **网站地址**: https://battle1king.github.io/SF_blog/

---

## 你还需要做的事情（首次部署清单）

### 1. 启用 GitHub Pages

1. 打开 https://github.com/battle1king/SF_blog/settings/pages
2. **Source** 选择 `Deploy from a branch`
3. **Branch** 选择 `gh-pages`，文件夹选 `/ (root)`
4. 点击 **Save**

> 如果还没有 `gh-pages` 分支，等第一次 Action 成功运行后会自动创建。

### 2. 确认 baseURL 已修改

确保 `config.toml` 和 `hugo.toml` 中的 `baseURL` 已改为：
```
https://battle1king.github.io/SF_blog/
```
> 如果以后绑定了自己的域名（如 `https://blog.example.com`），需要把 baseURL 改为你的域名。

### 3. 个性化配置

编辑 `config.toml`，修改以下内容：
- `title` — 网站标题
- `[author] name` — 你的名字
- `description` — 网站描述
- `[languages.en.menu.social]` — 你的社交链接
- `[languages.en.menu.links]` — 你的友链

---

## 日常工作流

### 写新文章

```bash
# 1. 创建新文章（会自动生成模板）
hugo new en/posts/my-new-post.md

# 2. 编辑文章
#    用 VS Code 打开 content/en/posts/my-new-post.md 进行编辑
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
date: 2026-03-12
draft: false
categories:
  - notes
tags:
  - Hugo
  - tutorial
---

正文内容从这里开始...
```

- `draft: true` → 草稿，不会发布
- `draft: false` → 正式发布

---

## 常用命令速查

| 命令 | 说明 |
|------|------|
| `hugo new en/posts/xxx.md` | 创建英文新文章 |
| `hugo new zh-tw/posts/xxx.md` | 创建繁体中文新文章 |
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
├── config.toml              # 主配置文件（网站设置、菜单、语言等）
├── hugo.toml                # Hugo 基础配置
├── content/                 # 文章内容
│   ├── en/                  # 英文内容
│   │   ├── about.md         # 关于页面
│   │   └── posts/           # 文章目录
│   └── zh-tw/               # 繁体中文内容
│       ├── about.md
│       └── posts/
├── static/                  # 静态资源（图片等）
│   └── images/              # 放你的图片
├── themes/                  # 主题（Git 子模块）
│   └── hugo-theme-den/
├── public/                  # 生成的静态文件（不需要手动修改）
├── .github/workflows/
│   └── gh-pages.yml         # GitHub Actions 自动部署配置
└── WORKFLOW.md              # 本文件
```

---

## 添加图片

1. 将图片放到 `static/images/` 目录下
2. 在文章中引用：
   ```markdown
   ![图片描述](/SF_blog/images/my-image.jpg)
   ```

---

## 常见问题

### Q: 推送后网站没更新？
1. 打开 https://github.com/battle1king/SF_blog/actions 查看 Actions 是否成功
2. 如果失败，点击进去查看错误日志

### Q: 本地预览正常但线上样式不对？
检查 `config.toml` 里的 `baseURL` 是否正确设置为 `https://battle1king.github.io/SF_blog/`

### Q: 文章写了但网站上看不到？
检查文章头部的 `draft` 是否设为 `false`

### Q: 如何绑定自定义域名？
1. 在 GitHub 仓库 Settings → Pages → Custom domain 填入你的域名
2. 在域名 DNS 中添加 CNAME 记录指向 `battle1king.github.io`
3. 修改 `config.toml` 和 `hugo.toml` 中的 `baseURL` 为你的域名
