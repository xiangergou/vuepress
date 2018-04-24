---
sidebar: auto
---

# 默认主题配置(default theme config)

::: tip 提示
此页面上列出的所有选项仅适用于默认主题。如果你使用的是自定义主题，则选项可能会有所不同。
:::

## 主页(Homepage)

默认主题提供了一个主页布局（用于[该网站的主页](/)）。要使用它，需要在你的根目录 `README.md` 的 [YAML front matter](../guide/markdown.html#yaml-front-matter) 中指定 `home：true`，并加上一些其他的元数据。这是本网站使用的实际数据：

``` yaml
---
home: true
heroImage: /hero.png
actionText: 起步 →
actionLink: /guide/
features:
- title: 简明优先
  details: 对以 markdown 为中心的项目结构，做最简化的配置，帮助你专注于创作。
- title: Vue 驱动
  details: 享用 Vue + webpack 开发环境，在 markdown 中使用 Vue 组件，并通过 Vue 开发自定义主题。
- title: 性能高效
  details: VuePress 将每个页面生成为预渲染的静态 HTML，每个页面加载之后，然后作为单页面应用程序(SPA)运行。
footer: MIT Licensed | Copyright © 2018-present Evan You
---
```

`YAML front matter` 的内容之后的其他任意内容，将被解析为正常 markdown，并在 features 部分之后渲染。

如果你想彻底自定义主页的布局，你还可以使用[自定义布局](#custom-layout-for-specific-pages)

## 导航链接(navbar links)

你可以通过 `themeConfig.nav` 将链接添加到导航栏中：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Guide', link: '/guide/' },
      { text: 'External', link: 'https://google.com' },
    ]
  }
}
```

如果你提供一个 `items` 而不是 `link` 的数组，这些链接也可以是下拉菜单：

```js
module.exports = {
  themeConfig: {
    nav: [
      {
        text: 'Languages',
        items: [
          { text: 'Chinese', link: '/language/chinese' },
          { text: 'Japanese', link: '/language/japanese' }
        ]
      }
    ]
  }
}
```

另外，你可以通过嵌套的 items 在下拉菜单中设置子分组：

```js
module.exports = {
  themeConfig: {
    nav: [
      {
        text: 'Languages',
        items: [
          { text: 'Group1', items: [/*  */] },
          { text: 'Group2', items: [/*  */] }
        ]
      }
    ]
  }
}
```

## 侧边栏(sidebar)

要启用侧边栏，请使用 `themeConfig.sidebar`。基本配置需要一系列链接：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
      '/',
      '/page-a',
      ['/page-b', 'Explicit link text']
    ]
  }
}
```

你可以省略 `.md` 扩展名，以 `/` 结尾的路径被推断为 `*/README.md` 。该链接的文本是自动推断的（从页面的第一个标题或 `YAML front matter` 中的显式标题）。如果你希望明确指定链接文本，请使用 `[link,text]` 形式的数组。

### 嵌套标题链接(nested header links)

侧边栏自动显示当前激活页面中标题的链接，嵌套在页面本身的链接下。你可以使用 `themeConfig.sidebarDepth` 自定义此行为。默认深度是 `1`，它提取 `h2` 标题。将其设置为 `0` 将禁用标题链接，最大值为`2`，同时提取 `h2` 和 `h3` 标题。

页面也可以在使用 `YAML front matter` 时覆盖此值：

``` md
---
sidebarDepth: 2
---
```

### 侧边栏分组(sidebar groups)

你可以使用对象将侧边栏链接分成多个分组：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
      {
        title: 'Group 1',
        collapsable: false,
        children: [
          '/'
        ]
      },
      {
        title: 'Group 2',
        children: [ /* ... */ ]
      }
    ]
  }
}
```

侧边栏分组默认情况下是可折叠的。你可以强制一个分组始终以 `collapsable：false` 打开。

### 多个侧边栏(multiple sidebars)

如果你希望为不同的页面分组显示不同的侧边栏，请先将页面组织到目录中：

```
.
├─ README.md
├─ foo
│  ├─ README.md
│  ├─ one.md
│  └─ two.md
└─ bar
   ├─ README.md
   ├─ three.md
   └─ four.md
```

然后，使用以下侧边栏配置：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: {
      // 侧边栏在 /foo/ 上
      '/foo/': [
        '',
        'one',
        'two'
      ],
      // 侧边栏在 /bar/ 上
      '/bar/': [
        '',
        'three',
        'four'
      ]
    }
  }
}
```

### 单页自动补充工具栏(auto sidebar for single pages)

如果你希望自动生成仅包含当前页面的标题链接的侧边栏，可以在该页面上使用  `YAML front matter`：

``` yaml
---
sidebar: auto
---
```

### 禁用侧边栏(disabling the sidebar)

你可以使用 `YAML front matter` 禁用特定页面上的侧边栏：

``` yaml
---
sidebar: false
---
```

## 上一页/下一页链接(prev/next links)

根据激活页面的侧边栏顺序自动推断上一个和下一个链接。你也可以使用 `YAML front matter` 来显式覆盖或禁用它们：

``` yaml
---
prev: ./some-other-page
next: false
---
```

## GitHub 仓库和编辑链接

提供 `themeConfig.repo` 会在导航栏中自动生成一个 GitHub 链接，并在每个页面的底部显示「编辑此页面」链接。

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    // 假定 GitHub。也可以是一个完整的 GitLab 网址
    repo: 'vuejs/vuepress',
    // 如果你的文档不在仓库的根部
    docsDir: 'docs',
    // 可选，默认为 master
    docsBranch: 'master',
    // 默认为 true，设置为 false 来禁用
    editLinks: true
    // custom text for edit link. Defaults to "Edit this page"
    editLinkText: 'Help us improve this page!'
  }
}
```

## 简单的 CSS 覆盖

如果你希望对默认主题的样式应用简单的覆盖，可以创建一个 `.vuepress/override.styl` 文件。 这是 [Stylus](http://stylus-lang.com/) 文件，但你也可以使用普通的 CSS 语法。

有几个颜色变量可以调整：

``` stylus
// 显示默认值
$accentColor = #3eaf7c
$textColor = #2c3e50
$borderColor = #eaecef
$codeBgColor = #282c34
```

## 自定义页面的 class

有时，你可能需要为特定的页面添加一个唯一的 class，以便你只能在自定义 CSS 中定位该页面上的内容。 你可以在 `YAML front matter` 中用 `pageClass` 在主题的容器 div 中添加一个 class：

``` yaml
---
pageClass: custom-page-class
---
```

然后你就可以只编写针对该页面的 CSS：

``` css
.theme-container.custom-page-class {
  /* 指定页面的规则 */
}
```

## 特定页面的自定义布局(custom layout for specific pages)

默认情况下，每个 `*.md` 文件的内容都会显示在一个 `<div class =“page”>` 容器中，以及侧边栏，自动生成的编辑链接和 prev/next 链接。如果你希望使用完全自定义的组件代替页面（同时只保留导航栏），则可以使用 `YAML front matter` 再次指定要使用的组件：

``` yaml
---
layout: SpecialLayout
---
```

这将为给定页面渲染 `.vuepress/components/SpecialLayout.vue`。

## Ejecting

你可以将默认主题的源代码复制到 `.vuepress/theme` 中，来使用 `vuepress eject [targetDir]` 命令彻底自定义主题。但是请注意，一旦你 eject，即使你升级 VuePress 版本，你这仍然是自己的主题，并且不会收到对默认主题的未来更新或错误修复。

***

> 原文：[https://vuepress.vuejs.org/default-theme-config/](https://vuepress.vuejs.org/default-theme-config/)

***
