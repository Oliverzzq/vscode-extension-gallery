---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: Prism
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
info: |
  ## 从零到一开发一款 VS Code 插件

  更多好文请查看 [洛竹的掘金](https://juejin.cn/user/325111174662855/posts)
# persist drawings in exports and build
drawings:
  persist: false
# 提供可下载的 PDF
download: 'https://cdn.jsdelivr.net/gh/youngjuning/vscode-extension-gallery@gh-pages/slidev-exported.pdf'
---

<link href="https://cdn.jsdelivr.net/npm/prism-themes@1.9.0/themes/prism-one-light.min.css" rel="stylesheet" />

# 从零到一开发一款 VS Code 插件

<div class="flex" style="justify-content:center">
  <img class="mr-10" src="https://cdn.jsdelivr.net/gh/youngjuning/images/202109211725265.png" width="180"/>
  <img src="https://cdn.jsdelivr.net/gh/youngjuning/images/202110141234963.jpeg" width="180"/>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/youngjuning/vscode-extension-gallery/tree/gh-pages" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---

# Hello Vs Code Extension

英雄多起于市井，高楼皆起于平地。再伟大的软件也都是从 Hello World 开始的，我们尽量用最简洁的步骤描述一个 vscode 插件 Hello World 的诞生。

## 工具介绍

- 🛠 **[Yeoman](https://yeoman.io/)** - 用于现代网络应用程序的网络脚手架工具
- 📝 **[generator-code](https://github.com/Microsoft/vscode-generator-code)** - 基于 Yeoman 的 VS Code 扩展模板

## 安装工具

```shell
npm install -g yo generator-code
```

## 生成项目

```shell
yo code
```

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# VS Code 扩展项目目录结构

```shell
.
├── CHANGELOG.md # 基于 standard-version 生成的更新日志文件
├── README.md # 项目描述文件，会展示在插件主页
├── package.json # vscode 包配置文件，诸如插件 LOGO、名字、描述、注册激活事件
├── src
│   └── extension.ts # 插件入口文件，暴露 activate 方法用于注册命令和初始化一些配置，暴露 deactivate 方法用于插件关闭前执行清理工作
├── tsconfig.json # vscode 的编译配置
└── yarn.lock
```

从目录结构可以看出，关键的文件是 package.json 和 extension.ts，下面，我们以 helloWorld 命令为例介绍下 vscode 插件的三个核心概念。

---

# Vs Code 扩展核心概念

1. 激活事件

激活事件是在 package.json 中的 `activationEvents` 字段声明的一个 JSON 数组对象。为了注册 helloWorld 这个命令，第一步就是注册激活事件，激活事件类型有很多，注册命令的激活事件是 `onCommand`。

2. 发布内容配置

发布内容配置（ 即 VS Code 为插件扩展提供的配置项）是 package.json 的 `contributes` 字段，你可以在其中注册各种配置项扩展 VS Code 的能力。上一步我们注册的 helloWorld 激活事件只是告诉了 vscode 可以通过 tuyaya.helloWorld 命令触发。我们还需要再 `contributes.commands` 中注册我们的 `tuyaya.helloWorld` 命令。

3. VS Code API

VS Code API 是 VS Code 提供给插件使用的一系列 Javascript API。通过前两个核心概念的能力，我们已经注册好了命令和事件，那么下一步必然就是注册事件回调。事件回调在 vscode 中是通过 `vscode.commands.registerCommand` 函数来注册的。

> 推荐：中文 API 翻译网上没有找到，可以我正在组织翻译的 [VS Code API 中文文档](https://vscode-api-cn.js.org/)

---

# 项目规范

项目规范分为代码规范和 Git 提交规范，完整的前端规范配置涉及 eslint、prettier、editorconfig、commitlint 等等工具，配置起来十分繁琐。我这里是将最佳实践封装成脚手架使用来节省时间。

## 代码规范

<br>

- [@luozhu/create-coding-style](https://github.com/youngjuning/luozhu/tree/main/packages/create-coding-style)

```shell
npx @luozhu/create-coding-style
```

<br>

## Git 提交规范

<br>

```shell
npx @luozhu/create-commitlint
```

---
layout: image-right
image: https://cdn.jsdelivr.net/gh/youngjuning/images/202110141613724.png
---

# 调试

按下 F5 开启调试会出现[扩展开发宿主]窗口，然后按 Command+Shift+P 组件键输入 Hello World 命令。如下图所示 vscode 弹出了 Hello World from *! 的提示。

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141612566.png)

---

# Vs Code 扩展国际化

我们已经知道 vscode 中的配置都是在 package.json 中，而配置的国际化是约定在 package.nls.json 和 package.nls.zh-cn.json 这种文件中编写。比如我们要在中英文环境下命令配置中英文版本，我们可以在 package.nls.json 中写：

```json
{
  "contributes.commands.tuyaya.helloWorld.title": "Hello World"
}
```

在 package.nls.zh-cn.json 写：

```json
{
  "contributes.commands.tuyaya.helloWorld.title": "你好世界"
}
```

然后 package.json 中按照 `%contributes.commands.tuyaya.helloWorld.title%` 的方式使用。

---

# 打包

- [vsce](https://github.com/microsoft/vscode-vsce)：VS Code 扩展管理工具
- 发布者（publisher）：打包和发布 VS Code 扩展需要创建一个发布者，如何创建请参考 [创建一个发布者](https://youngjuning.js.org/4b349879ced6/#创建一个发行方)

## 安装 vsce

```shell
npm install -g vsce
```

## 打包

```shell
vsce package
```

<br>

> 注意：默认的 README.md 需要修改之后才能打包成功。

<br>

> 注意：如果使用 Yarn，打包的时候可以使用 `--no-yarn` 选项忽略警告。

---

# 打包原理

<div grid="~ cols-2 gap-2" m="-t-2">
  <div>
    <p>VS Code 扩展的打包产物是一个以 vsix 结尾的文件：</p>
    <img border="rounded" src="https://cdn.jsdelivr.net/gh/youngjuning/images/202110141635184.png" width="180">
  </div>

  <div>
    <p>尝试用归档工具解压后得到如下目录文件夹：</p>
    <img border="rounded" src="https://cdn.jsdelivr.net/gh/youngjuning/images/202110141639437.png" width="280">
  </div>
</div>

我们可以看到编译后的文件夹 out 和其他一些文件是被直接压缩进安装包的，我们可以看到 .cz-config.js、.prettierrc.js 和 commitlint.config.js 这种开发时文件也被压缩了，运行插件完全用不到，这明显不合理。其实和其他插件体系一样，vscode 也提供了 .vscodeignore 来实现打包忽略配置，我们将以上无关文件忽略重新打包即可。

---
layout: image-right
image: https://cdn.jsdelivr.net/gh/youngjuning/images/202110141644980.png
---

# 打包原理

我们打开 extension.js 会发现引用了 vscode 这个包：

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141644572.png)

但是我们的安装包中并没有 node_modules，那么 vscode 这个包存在在哪里呢？阅读[源码](https://is.gd/33GTcH) 后，我们可以发现 vscode 通过底层 Node.js API 将 vscode 库挂载到 Node.js 中了。

> 注意：默认打包会将 node_modules 全量打包进安装包，我们可以使用 esbuild 优化，感兴趣的同学可以参考 [使用 esbuild 优化打包](https://juejin.cn/post/7000589186898231333#heading-8)。

---

# 插件推荐

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141828077.png)

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141829359.png)

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141830825.png)

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141831690.png)

![](https://cdn.jsdelivr.net/gh/youngjuning/images/202110141831137.png)

> 具体案例：[精准提效|如何开发一款 VS Code yarn.lock 预览插件](https://wiki.tuya-inc.com:7799/page/1448189203589378125)