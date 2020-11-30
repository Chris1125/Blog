---
title: "使用 Hugo + GitHub Pages 生成静态博客"
date: 2020-11-30T10:22:19+08:00
draft: false
---
### 环境介绍

- 基于 MacOS 系统
- 依赖软件：Homebrew，Git

### Hugo

#### 官方介绍

> ## The world’s fastest framework for building websites
>
> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.

#### 安装及使用方法

##### 1. 安装 Hugo

MacOS 下使用 Homebrew 安装：

`brew install hugo`

验证安装结果：

`hugo version # 查看 Hugo 版本`

##### 2. 创建新的站点

`hugo new site [path] # path 为站点生成的目标路径`

##### 3. 添加主题

Hugo 默认没有主题，需要在 `themes` 目录中手动添加一个主题，主题可以查阅[官方主题列表](https://themes.gohugo.io/)。

`git submodule add https://github.com/<theme-name>.git themes/<theme-name>`

然后将主题添加到站点配置中

`echo 'theme = "<theme-name>"' >> config.toml`

##### 4. 添加一篇文章

`hugo new posts/hello-world.md`

##### 5. 本地运行

执行`hugo server -D`，访问 <http://localhost:1313/> 进行本地预览

---

### GitHub Pages

#### 创建仓库

具体步骤可参考[官方教程](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site)

#### 将 Hugo 部署到 GitHub Pages 上

1. 克隆 GitHub Pages 项目到本地

   `git clone https://github.com/<user>/<user>.github.io.git`

2. 进入项目目录 `cd <user>.github.io`，将已经搭建好的 Hugo 项目复制过来，执行

   `git submodule add -b main https://github.com/<user>/<user>.github.io.git public`

3. 修改 Hugo 配置文件 `congif.toml`

   ```toml
   baseURL = "http://<user>.github.io/"
   languageCode = "en-us"
   title = "My New Hugo Site"
   theme = "<theme-name>"
   ```

4. 创建自动部署脚本

   ```bash
   #!/bin/sh
   
   # If a command fails then the deploy stops
   set -e
   
   printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"
   
   # Build the project.
   hugo # if using a theme, replace with `hugo -t <YOURTHEME>`
   
   # Go To Public folder
   cd public
   
   # Add changes to git.
   git add .
   
   # Commit changes.
   msg="rebuilding site $(date)"
   if [ -n "$*" ]; then
   	msg="$*"
   fi
   git commit -m "$msg"
   
   # Push source and build repos.
   git push origin main
   ```

5. 执行 `./deploy.sh "Your optional commit message"`，几分钟后即可访问 `https://<user>.github.io`。

---

### 参考链接：

- [Quick Start](https://gohugo.io/getting-started/quick-start/)
- [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
