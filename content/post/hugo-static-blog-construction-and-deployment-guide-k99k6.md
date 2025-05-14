---
title: Hugo静态博客搭建与部署指南
slug: hugo-static-blog-construction-and-deployment-guide-k99k6
url: /post/hugo-static-blog-construction-and-deployment-guide-k99k6.html
date: '2025-05-13 16:42:38+08:00'
lastmod: '2025-05-14 18:25:11+08:00'
toc: true
isCJKLanguage: true
---



# Hugo静态博客搭建与部署指南

# 使用Hugo搭建静态博客指南

## 一、环境准备

### 1. 安装Hugo

* **Windows**：

  1. 下载最新版：https://github.com/gohugoio/hugo/releases
  2. 解压到`C:\Hugo\bin`​
  3. 添加路径到系统环境变量
  4. 验证安装：`hugo version`​
* **macOS**：

  ```bash
  brew install hugo
  ```
* **Linux**：

  ```bash
  sudo snap install hugo
  # 或下载二进制文件
  wget https://github.com/gohugoio/hugo/releases/download/v0.125.0/hugo_0.125.0_linux-amd64.deb
  sudo dpkg -i hugo*.deb
  ```

### 2. 安装Git

从 https://git-scm.com/ 下载对应版本安装

## 二、创建新站点

```bash
hugo new site myblog
cd myblog
git init
```

## 三、安装主题（以Ananke为例）

```bash
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo 'theme = "ananke"' >> config.toml
```

## 四、配置网站

编辑`config.toml`​：

```toml
baseURL = "https://yourdomain.com/"
languageCode = "zh-CN"
title = "我的博客"
theme = "ananke"

[params]
  favicon = "favicon.ico"
  featured_image = "/images/default-bg.jpg"
```

## 五、创建内容

### 1. 新建文章

```bash
hugo new posts/first-post.md
```

编辑`content/posts/first-post.md`​：

```markdown
---
title: "我的第一篇文章"
date: 2024-03-20
draft: false
---

## 欢迎来到我的博客！

这是使用Hugo创建的第一篇文章...
```

### 2. 创建关于页面

```bash
hugo new about.md
```

## 六、本地预览

```bash
hugo server -D
```

访问 http://localhost:1313 查看效果

## 七、生成静态文件

```bash
hugo -D
```

生成的文件位于`public/`​目录

## 八、部署到GitHub Pages

1. 新建仓库 `username.github.io`​
2. 初始化发布目录：

    ```bash
    cd public
    git init
    git add .
    git commit -m "Initial commit"
    git branch -M main
    git remote add origin https://github.com/username/username.github.io.git
    git push -u origin main
    ```
3. 访问 https://username.github.io

## 九、自动部署（使用GitHub Actions）

1. 在项目根目录创建`.github/workflows/deploy.yml`​：

    ```yaml
    name: Deploy Hugo
    on:
      push:
        branches: [ main ]
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
            with:
              submodules: recursive
          - name: Setup Hugo
            uses: peaceiris/actions-hugo@v2
            with:
              hugo-version: 'latest'
          - name: Build
            run: hugo --minify
          - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
              github_token: ${{ secrets.GITHUB_TOKEN }}
              publish_dir: ./public
    ```

## 十、进阶配置

1. **自定义样式**：  
    在`assets/css/`​中添加自定义CSS文件

    ```toml
    [params]
      custom_css = ["css/custom.css"]
    ```
2. **添加评论系统**（使用Utterances）：  
    在文章模板`layouts/_default/single.html`​中添加：

    ```html
    <script src="https://utteranc.es/client.js"
            repo="username/repo"
            issue-term="pathname"
            theme="github-light"
            crossorigin="anonymous"
            async>
    </script>
    ```

## 常见问题

1. **文章不显示**：

    * 检查front matter中的`draft: false`​
    * 确认文件位于`content/posts/`​目录
2. **样式异常**：

    * 运行`hugo server --disableFastRender`​
    * 清除浏览器缓存
3. **部署后404错误**：

    * 检查`config.toml`​中的`baseURL`​
    * 确认GitHub仓库名为`username.github.io`​

## 维护建议

* 定期更新Hugo版本：`brew upgrade hugo`​
* 备份`content/`​和`config.toml`​文件
* 使用`hugo -D`​生成含草稿的测试版本

按照这个指南，您可以在30分钟内完成从零到部署的完整流程。Hugo官方文档（https://gohugo.io/documentation/）提供了更多高级功能的详细说明。
