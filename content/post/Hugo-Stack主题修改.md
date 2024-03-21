---
title: "Hugo Stack主题修改"
author: "Tom Almighty"
date: 2024-03-21T17:13:54+08:00
math: true
license: false
hidden: false
comments: true
draft: false
image: "https://pic.imgdb.cn/item/65fc10be9f345e8d0349611a.webp"
description: Stack 主题修改记录。
slug: "stack"
tags: ["Hugo"]
categories: ["折腾记录","日常生活","学习笔记"] 
---

**前言**：上一篇文章介绍了如何部署 Hugo 博客，这里针对 Stack 主题的修改做一些记录。

> 修改全部来自参考文章，这里仅记录。

**准备：**

首先在博客根目录下的 `assets` 下创建一个名为 `scss` 的文件夹,然后在 `scss` 文件夹里创建一个名为 `custom.scss` 的文件,最终效果为`~blog/assets/scss/custom.scss`,创建好文件后，对于主题的大部分样式魔改都将写进这个 `custom.scss`，其中有很多冗余的代码。

## 整体细节调整

```scss
//  ~\blog\assets\scss\custom.scss

// 页面基本配色
:root {
  // 全局顶部边距
  --main-top-padding: 30px;
  // 全局卡片圆角
  --card-border-radius: 25px;
  // 标签云卡片圆角
  --tag-border-radius: 8px;
  // 卡片间距
  --section-separation: 40px;
  // 全局字体大小
  --article-font-size: 1.8rem;
  // 行内代码背景色
  --code-background-color: #f8f8f8;
  // 行内代码前景色
  --code-text-color: #e96900;
  // 暗色模式下样式
  &[data-scheme="dark"] {
    // 行内代码背景色
    --code-background-color: #ff6d1b17;
    // 行内代码前景色
    --code-text-color: #e96900;
    // 暗黑模式下背景色
    --body-background: #000;
    // 暗黑模式下卡片背景色
    --card-background: hsl(225 13% 8% / 1);
  }
}
//------------------------------------------------------
// 修复引用块内容窄页面显示问题
a {
  word-break: break-all;
}

code {
  word-break: break-all;
}

//---------------------------------------------------
// 文章封面高度
.article-list article .article-image img {
  width: 100%;
  height: 200px !important;
  object-fit: cover;

  @include respond(md) {
      height: 250px !important;
  }

  @include respond(xl) {
      height: 285px !important;
  }
}

//--------------------------------------------------
// 文章内容图片圆角阴影
.article-page .main-article .article-content {
  img {
    max-width: 96% !important;
    height: auto !important;
    border-radius: 8px;
  }
}

//------------------------------------------------
// 文章内容引用块样式
.article-content {
  blockquote {
    border-left: 6px solid #358b9a1f !important;
    background: #3a97431f;
  }
}
// ---------------------------------------
// 代码块样式修改
.highlight {
  max-width: 102% !important;
  background-color: var(--pre-background-color);
  padding: var(--card-padding);
  position: relative;
  border-radius: 20px;
  margin-left: -7px !important;
  margin-right: -12px;
  box-shadow: var(--shadow-l1) !important;

  &:hover {
    .copyCodeButton {
      opacity: 1;
    }
  }

  // keep Codeblocks LTR
  [dir="rtl"] & {
    direction: ltr;
  }

  pre {
    margin: initial;
    padding: 0;
    margin: 0;
    width: auto;
  }
}

// light模式下的代码块样式调整
[data-scheme="light"] .article-content .highlight {
  background-color: #fff9f3;
}

[data-scheme="light"] .chroma {
  color: #ff6f00;
  background-color: #fff9f3cc;
}

//-------------------------------------------
// 设置选中字体的区域背景颜色
//修改选中颜色
::selection {
  color: #fff;
  background: #34495e;
}

a {
  text-decoration: none;
  color: var(--accent-color);

  &:hover {
    color: var(--accent-color-darker);
  }

  &.link {
    color: #4288b9ad;
    font-weight: 600;
    padding: 0 2px;
    text-decoration: none;
    cursor: pointer;

    &:hover {
      text-decoration: underline;
    }
  }
}

//-------------------------------------------------
//文章封面高度更改
.article-list article .article-image img {
  width: 100%;
  height: 150px;
  object-fit: cover;

  @include respond(md) {
    height: 200px;
  }

  @include respond(xl) {
    height: 305px;
  }
}

//---------------------------------------------------
// 全局页面布局间距调整
.main-container {
  min-height: 100vh;
  align-items: flex-start;
  padding: 0 15px;
  gap: var(--section-separation);
  padding-top: var(--main-top-padding);

  @include respond(md) {
    padding: 0 37px;
  }
}

//--------------------------------------------------
//页面三栏宽度调整
.container {
  margin-left: auto;
  margin-right: auto;

  .left-sidebar {
    order: -3;
    max-width: var(--left-sidebar-max-width);
  }

  .right-sidebar {
    order: -1;
    max-width: var(--right-sidebar-max-width);

    /// Display right sidebar when min-width: lg
    @include respond(lg) {
      display: flex;
    }
  }

  &.extended {
    @include respond(md) {
      max-width: 1024px;
      --left-sidebar-max-width: 25%;
      --right-sidebar-max-width: 22% !important;
    }

    @include respond(lg) {
      max-width: 1280px;
      --left-sidebar-max-width: 20%;
      --right-sidebar-max-width: 30%;
    }

    @include respond(xl) {
      max-width: 1453px; //1536px;
      --left-sidebar-max-width: 15%;
      --right-sidebar-max-width: 25%;
    }
  }

  &.compact {
    @include respond(md) {
      --left-sidebar-max-width: 25%;
      max-width: 768px;
    }

    @include respond(lg) {
      max-width: 1024px;
      --left-sidebar-max-width: 20%;
    }

    @include respond(xl) {
      max-width: 1280px;
    }
  }
}

//-------------------------------------------------------
//全局页面小图片样式微调
.article-list--compact article .article-image img {
  width: var(--image-size);
  height: var(--image-size);
  object-fit: cover;
  border-radius: 17%;
}

//----------------------------------------------------
//固定代码块的高度
.article-content {
  .highlight {
      padding: var(--card-padding);
      pre {
          width: auto;
          max-height: 20em;
      }
  }
}

//--------------------------------------------------
// 修改首页搜索框样式
.search-form.widget input {
  font-size: 1.5rem;
  padding: 44px 25px 19px;
}
```

## 菜单栏调整为圆角

```scss
//  ~\blog\assets\scss\custom.scss

// --------------------------------
// 菜单栏样式
// 下拉菜单改圆角样式
.menu {
  padding-left: 0;
  list-style: none;
  flex-direction: column;
  overflow-x: hidden;
  overflow-y: scroll;
  flex-grow: 1;
  font-size: 1.6rem;
  background-color: var(--card-background);

  box-shadow: var(--shadow-l2); //改个阴影
  display: none;
  margin: 0; //改为0
  border-radius: 10px; //加个圆角
  padding: 30px 30px;

  @include respond(xl) {
    padding: 15px 0;
  }

  &,
  .menu-bottom-section {
    gap: 30px;

    @include respond(xl) {
      gap: 25px;
    }
  }

  &.show {
    display: flex;
  }

  @include respond(md) {
    align-items: flex-end;
    display: flex;
    background-color: transparent;
    padding: 0;
    box-shadow: none;
    margin: 0;
  }

  li {
    position: relative;
    vertical-align: middle;
    padding: 0;

    @include respond(md) {
      width: 100%;
    }

    svg {
      stroke-width: 1.33;

      width: 20px;
      height: 20px;
    }

    a {
      height: 100%;
      display: inline-flex;
      align-items: center;
      color: var(--body-text-color);
      gap: var(--menu-icon-separation);
    }

    span {
      flex: 1;
    }

    &.current {
      a {
        color: var(--accent-color);
        font-weight: bold;
      }
    }
  }
}
```

## 滚动条美化

```scss
//  ~\blog\assets\scss\custom.scss

//------------------------------------------------
//将滚动条修改为圆角样式
//菜单滚动条美化
.menu::-webkit-scrollbar {
  display: none;
}

// 全局滚动条美化
html {
  ::-webkit-scrollbar {
    width: 20px;
  }

  ::-webkit-scrollbar-track {
    background-color: transparent;
  }

  ::-webkit-scrollbar-thumb {
    background-color: #d6dee1;
    border-radius: 20px;
    border: 6px solid transparent;
    background-clip: content-box;
  }

  ::-webkit-scrollbar-thumb:hover {
    background-color: #a8bbbf;
  }
}
```

## 归档页双栏

```scss
//  ~\blog\assets\scss\custom.scss

//--------------------------------------------------
//归档页面双栏
/* 归档页面两栏 */
@media (min-width: 1024px) {
  .article-list--compact {
    display: grid;
    grid-template-columns: 1fr 1fr;
    background: none;
    box-shadow: none;
    gap: 1rem;

    article {
      background: var(--card-background);
      border: none;
      box-shadow: var(--shadow-l2);
      margin-bottom: 8px;
      border-radius: 16px;
    }
  }
}
```

## 链接页三栏

```scss
//  ~\blog\assets\scss\custom.scss

//--------------------------------------------------
//链接三栏
@media (min-width: 1024px) {
  .article-list--compact.links {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr; //三个1fr即为三栏,两个1fr则为双栏,以此类推即可.
    background: none;
    box-shadow: none;
    gap: 1rem;

    article {
      background: var(--card-background);
      border: none;
      box-shadow: var(--shadow-l2);
      margin-bottom: 8px;
      border-radius: var(--card-border-radius);

      &:nth-child(odd) {
        margin-right: 8px;
      }
    }
  }
}
```

## 首页添加欢迎样式

在博客的根目录下新建一个文件夹名为 `layouts` (默认生成站点时也生成了，没有的话手动创建),之后将 `~\blog\themes\hugo-theme-stack\layouts\index.html` 下的文件复制到刚刚创建的 `layouts` 文件夹里,这意味着主题 根目录下的 `layouts`文件夹里的 `index.html`将覆盖原主题目录下对应的文件，然后在复制出来的`index.html`修改为以下内容：

```scss
<!-- ~\site\blog\layouts\index.html -->
{{ define "main" }}
    {{ $pages := where .Site.RegularPages "Type" "in" .Site.Params.mainSections }}
    {{ $notHidden := where .Site.RegularPages "Params.hidden" "!=" true }}
    {{ $filtered := ($pages | intersect $notHidden) }}
    {{ $pag := .Paginate ($filtered) }}
<!-- ---这是我们添加进去的--------- -->
<!-- 首页欢迎字幅板块 -->
<div class="welcome">
  <p style="font-size: 2rem; text-align: center; font-weight: bold">
    <span class="shake">👋</span>
    <span class="jump-text1" > Welcome</span>
    <span class="jump-text2"> To </span>
    <span class="jump-text3" style="color:#e99312">Tom</span>
    <span class="jump-text4" style="color:#e99312">'s</span>
    <span class="jump-text5" style="color:#e99312">Blog</span>
  </p>
</div>
<!-- ------首页欢迎字幅板块------ -->
<!-- 下面也是主题自带的,只展示一部分,其余省略 -->

    <section class="article-list">
        {{ range $index, $element := $pag.Pages }}
            {{ partial "article-list/default" . }}
        {{ end }}
    </section>

    {{- partial "pagination.html" . -}}
    {{- partial "footer/footer" . -}}
{{ end }}

{{ define "right-sidebar" }}
    {{ partial "sidebar/right.html" (dict "Context" . "Scope" "homepage") }}
{{ end }}

```

接下来在 `custom.scss` 中添加如下：
```scss
//  ~\blog\assets\scss\custom.scss

//---------------------------------------------------------
//首页欢迎板块样式
.welcome {
  color: var(--card-text-color-main);
  background: var(--card-background);
  box-shadow: var(--shadow-l2);
  border-radius: 30px;
  display: inline-block;
}

// 👋emoji实现摆动效果
.shake {
  display: inline-block;
  animation: shake 1s;
  animation-duration: 1s;
  animation-timing-function: ease;
  animation-delay: 0s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-fill-mode: none;
  animation-play-state: running;
  animation-name: shake;
  animation-timeline: auto;
  animation-range-start: normal;
  animation-range-end: normal;
  animation-delay: 2s;
  @keyframes shake {
    0% {
      transform: rotate(0);
    }
    25% {
      transform: rotate(45deg) scale(1.2);
    }
    50% {
      transform: rotate(0) scale(1.2);
    }
    75% {
      transform: rotate(45deg) scale(1.2);
    }
    100% {
      transform: rotate(0);
    }
  }
}
// 实现字符跳动动画
.jump-text1 {
  display: inline-block;
  animation: jump 0.5s 1;
}

.jump-text2 {
  display: inline-block;
  animation: jump 0.5s 1;
  animation-delay: 0.1s;
}

.jump-text3 {
  display: inline-block;
  animation: jump 0.5s 1;
  animation-delay: 0.2s;
}

.jump-text4 {
  display: inline-block;
  animation: jump 0.5s 1;
  animation-delay: 0.3s;
}

.jump-text5 {
  display: inline-block;
  animation: jump 0.5s 1;
  animation-delay: 0.4s;
}

// .jump-text6 {
//   display: inline-block;
//   animation: jump 0.5s 1;
//   animation-delay: 0.5s;
// }

// .jump-text7 {
//   display: inline-block;
//   animation: jump 0.5s 1;
//   animation-delay: 0.6s;
// }

// .jump-text8 {
//   display: inline-block;
//   animation: jump 0.5s 1;
//   animation-delay: 0.7s;
// }

// .jump-text9 {
//   display: inline-block;
//   animation: jump 0.5s 1;
//   animation-delay: 0.9s;
// }

@keyframes jump {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(0);
  }
}
```



## 代码块引入 Mac 样式

首先在博客根目录下的 `static` 文件夹中创建名为 `img` 的文件夹,然后在 `img` 文件夹中创建一个名为 `code-header.svg` 的文件,在文件中写入以下内容:

```svg
<svg xmlns="http://www.w3.org/2000/svg" version="1.1"  x="0px" y="0px" width="450px" height="130px">
    <ellipse cx="65" cy="65" rx="50" ry="52" stroke="rgb(220,60,54)" stroke-width="2" fill="rgb(237,108,96)"/>
    <ellipse cx="225" cy="65" rx="50" ry="52"  stroke="rgb(218,151,33)" stroke-width="2" fill="rgb(247,193,81)"/>
    <ellipse cx="385" cy="65" rx="50" ry="52"  stroke="rgb(27,161,37)" stroke-width="2" fill="rgb(100,200,86)"/>
</svg>
```

接下来继续在 `custom.scss` 添加以下内容：

```scss
//  ~\blog\assets\scss\custom.scss

//----------------------------------------------------------
//为代码块顶部添加macos样式
.article-content {
  .highlight:before {
    content: "";
    display: block;
    background: url(/img/code-header.svg);
    height: 32px;
    width: 100%;
    background-size: 57px;
    background-repeat: no-repeat;
    margin-bottom: 5px;
    background-position: -1px 2px;
  }
}
```

接下来在代码框右上角添加代码语言，并修改复制按钮文字：

在`themes/assets/ts/main.ts`  （也可以复制到站点根目录修改）中，第 `60` 行后（`observer.observe(articleTile)`下一行的括号后，`window.addEventListener('load', () => {`之前），添加以下代码：

```ts
        /**
         * Add copy button to code block
        */
        const highlights = document.querySelectorAll(".article-content div.highlight");
        const copyText = `📄拷贝`,
            copiedText = `已拷贝!`;
        
        highlights.forEach((highlight) => {
            const copyButton = document.createElement("button");
            copyButton.innerHTML = copyText;
            copyButton.classList.add("copyCodeButton");
            highlight.appendChild(copyButton);
        
            const codeBlock = highlight.querySelector("code[data-lang]");
            // 获取语言
            const lang = codeBlock.getAttribute("data-lang");
            if (!codeBlock) return;
        
            copyButton.addEventListener("click", () => {
                navigator.clipboard
                    .writeText(codeBlock.textContent)
                    .then(() => {
                        copyButton.textContent = copiedText;
        
                        setTimeout(() => {
                            copyButton.textContent = copyText;
                        }, 1000);
                    })
                    .catch((err) => {
                        alert(err);
                        console.log("Something went wrong", err);
                    });
            });
        
            // Add language code button
            const languageButton = document.createElement("button");
            languageButton.innerHTML = lang.toUpperCase() + "&nbsp;&nbsp;";
            languageButton.classList.add("languageCodeButton");
        
            highlight.appendChild(languageButton);
        });
        
        new StackColorScheme(document.getElementById("dark-mode-toggle"));
        
    }
}
```

接下来在 `custom.scss` 中添加如下：

```scss
//代码复制按钮
.article-content .copyCodeButton {
  position: absolute;
  top: 10px;
  right: 18px;
  border-radius: 12px;
  opacity: 1;
  color: #F44336;
  background: none;
  border: none;
  padding: 0;
  font-weight: 500;
}

.article-content .languageCodeButton {
  position: absolute;
  border: none;
  top: 9px;
  right: 69px;
  border-radius: 12px;
  opacity: 1;
  padding: 0 5px;
  background: 0;
  color: #F44336;
  font-family: lato;
  font-size: 1.5rem;
}
```



## 参考

- [L1nSn0w's Blog](https://blog.linsnow.cn/p/modify-hugo/)
- [Lovir's Blog](https://lovir.cn/p/changes-in-my-blog/)