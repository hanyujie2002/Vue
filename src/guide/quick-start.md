---
footer: false
---

<script setup>
import { VTCodeGroup, VTCodeGroupTab } from '@vue/theme'
</script>

# 快速上手 | Quick Start {#quick-start}

## 线上尝试 Vue | Try Vue Online {#try-vue-online}

- To quickly get a taste of Vue, you can try it directly in our [Playground](https://play.vuejs.org/#eNo9jcEKwjAMhl/lt5fpQYfXUQfefAMvvRQbddC1pUuHUPrudg4HIcmXjyRZXEM4zYlEJ+T0iEPgXjn6BB8Zhp46WUZWDjCa9f6w9kAkTtH9CRinV4fmRtZ63H20Ztesqiylphqy3R5UYBqD1UyVAPk+9zkvV1CKbCv9poMLiTEfR2/IXpSoXomqZLtti/IFwVtA9A==).

- 想要快速体验 Vue，你可以直接试试我们的[演练场](https://play.vuejs.org/#eNo9jcEKwjAMhl/lt5fpQYfXUQfefAMvvRQbddC1pUuHUPrudg4HIcmXjyRZXEM4zYlEJ+T0iEPgXjn6BB8Zhp46WUZWDjCa9f6w9kAkTtH9CRinV4fmRtZ63H20Ztesqiylphqy3R5UYBqD1UyVAPk+9zkvV1CKbCv9poMLiTEfR2/IXpSoXomqZLtti/IFwVtA9A==)。

- If you prefer a plain HTML setup without any build steps, you can use this [JSFiddle](https://jsfiddle.net/yyx990803/2ke1ab0z/) as your starting point.

- 如果你更喜欢不用任何构建的原始 HTML，可以使用 [JSFiddle](https://jsfiddle.net/yyx990803/2ke1ab0z/) 入门。

- If you are already familiar with Node.js and the concept of build tools, you can also try a complete build setup right within your browser on [StackBlitz](https://vite.new/vue).

- 如果你已经比较熟悉 Node.js 和构建工具等概念，还可以直接在浏览器中打开 [StackBlitz](https://vite.new/vue) 来尝试完整的构建设置。

## 创建一个 Vue 应用 | Creating a Vue Application {#creating-a-vue-application}

:::tip 前提条件 | Prerequisites

- Familiarity with the command line
- 熟悉命令行
- Install [Node.js](https://nodejs.org/) version 18.3 or higher
- 已安装 18.3 或更高版本的 [Node.js](https://nodejs.org/)
:::

In this section we will introduce how to scaffold a Vue [Single Page Application](/guide/extras/ways-of-using-vue#single-page-application-spa) on your local machine. The created project will be using a build setup based on [Vite](https://vitejs.dev) and allow us to use Vue [Single-File Components](/guide/scaling-up/sfc) (SFCs).

在本节中，我们将介绍如何在本地搭建 Vue [单页应用](/guide/extras/ways-of-using-vue#single-page-application-spa)。创建的项目将使用基于 [Vite](https://vitejs.dev) 的构建设置，并允许我们使用 Vue 的[单文件组件](/guide/scaling-up/sfc) (SFC)。

Make sure you have an up-to-date version of [Node.js](https://nodejs.org/) installed and your current working directory is the one where you intend to create a project. Run the following command in your command line (without the `$` sign):

确保你安装了最新版本的 [Node.js](https://nodejs.org/)，并且你的当前工作目录正是打算创建项目的目录。在命令行中运行以下命令 (不要带上 `$` 符号)：

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh
  $ npm create vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh
  $ pnpm create vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh
  # For Yarn Modern (v2+)
  $ yarn create vue@latest
  
  # For Yarn ^v4.11
  $ yarn dlx create-vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh
  $ bun create vue@latest
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

This command will install and execute [create-vue](https://github.com/vuejs/create-vue), the official Vue project scaffolding tool. You will be presented with prompts for several optional features such as TypeScript and testing support:

这一指令将会安装并执行 [create-vue](https://github.com/vuejs/create-vue)，它是 Vue 官方的项目脚手架工具。你将会看到一些诸如 TypeScript 和测试支持之类的可选功能提示：

<div class="language-sh"><pre><code><span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Project name: <span style="color:#888;">… <span style="color:#89DDFF;">&lt;</span><span style="color:#888;">your-project-name</span><span style="color:#89DDFF;">&gt;</span></span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add TypeScript? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add JSX Support? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Vue Router for Single Page Application development? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Pinia for state management? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Vitest for Unit testing? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add an End-to-End Testing Solution? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Cypress / Nightwatch / Playwright</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add ESLint for code quality? <span style="color:#888;">… No / <span style="color:#89DDFF;text-decoration:underline">Yes</span></span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Prettier for code formatting? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Vue DevTools 7 extension for debugging? (experimental) <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span></span>
<span style="color:#A6ACCD;">Scaffolding project in ./<span style="color:#89DDFF;">&lt;</span><span style="color:#888;">your-project-name</span><span style="color:#89DDFF;">&gt;</span>...</span>
<span style="color:#A6ACCD;">Done.</span></code></pre></div>

If you are unsure about an option, simply choose `No` by hitting enter for now. Once the project is created, follow the instructions to install dependencies and start the dev server:

如果不确定是否要开启某个功能，你可以直接按下回车键选择 `No`。在项目被创建后，通过以下步骤安装依赖并启动开发服务器：

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh-vue
  $ cd {{'<your-project-name>'}}
  $ npm install
  $ npm run dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh-vue
  $ cd {{'<your-project-name>'}}
  $ pnpm install
  $ pnpm run dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh-vue
  $ cd {{'<your-project-name>'}}
  $ yarn
  $ yarn dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh-vue
  $ cd {{'<your-project-name>'}}
  $ bun install
  $ bun run dev
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

You should now have your first Vue project running! Note that the example components in the generated project are written using the [Composition API](/guide/introduction#composition-api) and `<script setup>`, rather than the [Options API](/guide/introduction#options-api). Here are some additional tips:
你现在应该已经运行起来了你的第一个 Vue 项目！请注意，生成的项目中的示例组件使用的是[组合式 API](/guide/introduction#composition-api) 和 `<script setup>`，而非[选项式 API](/guide/introduction#options-api)。下面是一些补充提示：
- The recommended IDE setup is [Visual Studio Code](https://code.visualstudio.com/) + [Vue - Official extension](https://marketplace.visualstudio.com/items?itemName=Vue.volar). If you use other editors, check out the [IDE support section](/guide/scaling-up/tooling#ide-support).
- 推荐的 IDE 配置是 [Visual Studio Code](https://code.visualstudio.com/) + [Vue - Official 扩展](https://marketplace.visualstudio.com/items?itemName=Vue.volar)。如果使用其他编辑器，参考 [IDE 支持章节](/guide/scaling-up/tooling#ide-support)。
- More tooling details, including integration with backend frameworks, are discussed in the [Tooling Guide](/guide/scaling-up/tooling).
- 更多工具细节，包括与后端框架的整合，我们会在[工具链指南](/guide/scaling-up/tooling)进行讨论。
- To learn more about the underlying build tool Vite, check out the [Vite docs](https://vitejs.dev).
- 要了解构建工具 Vite 更多背后的细节，请查看 [Vite 文档](https://cn.vitejs.dev)。
- If you choose to use TypeScript, check out the [TypeScript Usage Guide](typescript/overview).
- 如果你选择使用 TypeScript，请阅读 [TypeScript 使用指南](typescript/overview)。

When you are ready to ship your app to production, run the following:

当你准备将应用发布到生产环境时，请运行：

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh
  $ npm run build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh
  $ pnpm run build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh
  $ yarn build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh
  $ bun run build
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

This will create a production-ready build of your app in the project's `./dist` directory. Check out the [Production Deployment Guide](/guide/best-practices/production-deployment) to learn more about shipping your app to production.

此命令会在 `./dist` 文件夹中为你的应用创建一个生产环境的构建版本。关于将应用上线生产环境的更多内容，请阅读[生产环境部署指南](/guide/best-practices/production-deployment)。

[下一步> | Next Steps >](#next-steps)

## 通过 CDN 使用 Vue | Using Vue from CDN {#using-vue-from-cdn}

You can use Vue directly from a CDN via a script tag:

你可以借助 script 标签直接通过 CDN 来使用 Vue：

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

Here we are using [unpkg](https://unpkg.com/), but you can also use any CDN that serves npm packages, for example [jsdelivr](https://www.jsdelivr.com/package/npm/vue) or [cdnjs](https://cdnjs.com/libraries/vue). Of course, you can also download this file and serve it yourself.

这里我们使用了 [unpkg](https://unpkg.com/)，但你也可以使用任何提供 npm 包服务的 CDN，例如 [jsdelivr](https://www.jsdelivr.com/package/npm/vue) 或 [cdnjs](https://cdnjs.com/libraries/vue)。当然，你也可以下载此文件并自行提供服务。

When using Vue from a CDN, there is no "build step" involved. This makes the setup a lot simpler, and is suitable for enhancing static HTML or integrating with a backend framework. However, you won't be able to use the Single-File Component (SFC) syntax.

通过 CDN 使用 Vue 时，不涉及“构建步骤”。这使得设置更加简单，并且可以用于增强静态的 HTML 或与后端框架集成。但是，你将无法使用单文件组件 (SFC) 语法。

### 使用全局构建版本 | Using the Global Build {#using-the-global-build}

The above link loads the _global build_ of Vue, where all top-level APIs are exposed as properties on the global `Vue` object. Here is a full example using the global build:

上面的链接使用了*全局构建版本*的 Vue，该版本的所有顶层 API 都以属性的形式暴露在了全局的 `Vue` 对象上。这里有一个使用全局构建版本的例子：

<div class="options-api">

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp } = Vue
  
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
```

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/QWJwJLp)

</div>

<div class="composition-api">

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp, ref } = Vue

  createApp({
    setup() {
      const message = ref('Hello vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/eYQpQEG)

:::tip
Many of the examples for Composition API throughout the guide will be using the `<script setup>` syntax, which requires build tools. If you intend to use Composition API without a build step, consult the usage of the [`setup()` option](/api/composition-api-setup).

本指南中许多关于组合式 API 的例子将使用 `<script setup>` 语法，这需要构建工具。如果你打算在没有构建步骤的情况下使用组合式 API，请参考 [`setup()` 选项](/api/composition-api-setup)的用法。
:::

</div>

### 使用 ES 模块构建版本 | Using the ES Module Build {#using-the-es-module-build}

Throughout the rest of the documentation, we will be primarily using [ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) syntax. Most modern browsers now support ES modules natively, so we can use Vue from a CDN via native ES modules like this:

在本文档的其余部分我们使用的主要是 [ES 模块](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules)语法。现代浏览器大多都已原生支持 ES 模块。因此我们可以像这样通过 CDN 以及原生 ES 模块使用 Vue：

<div class="options-api">

```html{3,4}
<div id="app">{{ message }}</div>

<script type="module">
  import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'
  
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
```

</div>

<div class="composition-api">

```html{3,4}
<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

</div>

Notice that we are using `<script type="module">`, and the imported CDN URL is pointing to the **ES modules build** of Vue instead.

注意我们使用了 `<script type="module">`，且导入的 CDN URL 指向的是 Vue 的 **ES 模块构建版本**。

<div class="options-api">

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/VwVYVZO)

</div>
<div class="composition-api">

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/MWzazEv)

</div>

### 启用 Import maps | Enabling Import maps {#enabling-import-maps}

In the above example, we are importing from the full CDN URL, but in the rest of the documentation you will see code like this:

在上面的示例中，我们使用了完整的 CDN URL 来导入，但在文档的其余部分中，你将看到如下代码：

```js
import { createApp } from 'vue'
```

We can teach the browser where to locate the `vue` import by using [Import Maps](https://caniuse.com/import-maps):

我们可以使用[导入映射表 (Import Maps)](https://caniuse.com/import-maps) 来告诉浏览器如何定位到导入的 `vue`：

<div class="options-api">

```html{1-7,12}
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp } from 'vue'

  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
```

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/wvQKQyM)

</div>

<div class="composition-api">

```html{1-7,12}
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'vue'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

[CodePen 示例 > | CodePen Demo >](https://codepen.io/vuejs-examples/pen/YzRyRYM)

</div>

You can also add entries for other dependencies to the import map - but make sure they point to the ES modules version of the library you intend to use.

你也可以在映射表中添加其他的依赖——但请务必确保你使用的是该库的 ES 模块版本。

:::tip 导入映射表的浏览器支持情况 | Import Maps Browser Support
Import Maps is a relatively new browser feature. Make sure to use a browser within its [support range](https://caniuse.com/import-maps). In particular, it is only supported in Safari 16.4+.

导入映射表是一个相对较新的浏览器功能。请确保使用其[支持范围](https://caniuse.com/import-maps)内的浏览器。请注意，只有 Safari 16.4 以上版本支持。
:::

:::warning 生产环境中的注意事项 | Notes on Production Use
The examples so far are using the development build of Vue - if you intend to use Vue from a CDN in production, make sure to check out the [Production Deployment Guide](/guide/best-practices/production-deployment#without-build-tools).

到目前为止示例中使用的都是 Vue 的开发构建版本——如果你打算在生产中通过 CDN 使用 Vue，请务必查看[生产环境部署指南](/guide/best-practices/production-deployment#without-build-tools)。

While it is possible to use Vue without a build system, an alternative approach to consider is using [`vuejs/petite-vue`](https://github.com/vuejs/petite-vue) that could better suit the context where [`jquery/jquery`](https://github.com/jquery/jquery) (in the past) or [`alpinejs/alpine`](https://github.com/alpinejs/alpine) (in the present) might be used instead.

虽然 Vue 可以不依赖构建系统使用，但也可以考虑使用 [`vuejs/petite-vue`](https://github.com/vuejs/petite-vue) 这个替代方案，以更好地适配可能在 [`jquery/jquery`](https://github.com/jquery/jquery) (过去) 或 [`alpinejs/alpine`](https://github.com/alpinejs/alpine) (现在) 的上下文中使用的情况。
:::

### 拆分模块 | Splitting Up the Modules {#splitting-up-the-modules}

As we dive deeper into the guide, we may need to split our code into separate JavaScript files so that they are easier to manage. For example:

随着对这份指南的逐步深入，我们可能需要将代码分割成单独的 JavaScript 文件，以便更容易管理。例如：

```html
<!-- index.html -->
<div id="app"></div>

<script type="module">
  import { createApp } from 'vue'
  import MyComponent from './my-component.js'

  createApp(MyComponent).mount('#app')
</script>
```

<div class="options-api">

```js
// my-component.js
export default {
  data() {
    return { count: 0 }
  },
  template: `<div>Count is: {{ count }}</div>`
}
```

</div>
<div class="composition-api">

```js
// my-component.js
import { ref } from 'vue'
export default {
  setup() {
    const count = ref(0)
    return { count }
  },
  template: `<div>Count is: {{ count }}</div>`
}
```

</div>

If you directly open the above `index.html` in your browser, you will find that it throws an error because ES modules cannot work over the `file://` protocol, which is the protocol the browser uses when you open a local file.

如果直接在浏览器中打开了上面的 `index.html`，你会发现它抛出了一个错误，因为 ES 模块不能通过 `file://` 协议工作，也即是当你打开一个本地文件时浏览器使用的协议。

Due to security reasons, ES modules can only work over the `http://` protocol, which is what the browsers use when opening pages on the web. In order for ES modules to work on our local machine, we need to serve the `index.html` over the `http://` protocol, with a local HTTP server.

由于安全原因，ES 模块只能通过 `http://` 协议工作，也即是浏览器在打开网页时使用的协议。为了使 ES 模块在我们的本地机器上工作，我们需要使用本地的 HTTP 服务器，通过 `http://` 协议来提供 `index.html`。

To start a local HTTP server, first make sure you have [Node.js](https://nodejs.org/en/) installed, then run `npx serve` from the command line in the same directory where your HTML file is. You can also use any other HTTP server that can serve static files with the correct MIME types.

要启动一个本地的 HTTP 服务器，请先安装 [Node.js](https://nodejs.org/zh/)，然后通过命令行在 HTML 文件所在文件夹下运行 `npx serve`。你也可以使用其他任何可以基于正确的 MIME 类型服务静态文件的 HTTP 服务器。

You may have noticed that the imported component's template is inlined as a JavaScript string. If you are using VS Code, you can install the [es6-string-html](https://marketplace.visualstudio.com/items?itemName=Tobermory.es6-string-html) extension and prefix the strings with a `/*html*/` comment to get syntax highlighting for them.

可能你也注意到了，这里导入的组件模板是内联的 JavaScript 字符串。如果你正在使用 VS Code，你可以安装 [es6-string-html](https://marketplace.visualstudio.com/items?itemName=Tobermory.es6-string-html) 扩展，然后在字符串前加上一个前缀注释 `/*html*/` 以高亮语法。

## 下一步 | Next Steps {#next-steps}

If you skipped the [Introduction](/guide/introduction), we strongly recommend reading it before moving on to the rest of the documentation.

如果你尚未阅读[简介](/guide/introduction)，我们强烈推荐你在移步到后续文档之前返回去阅读一下。

<div class="vt-box-container next-steps">
  <a class="vt-box" href="/guide/essentials/application.html">
    <p class="next-steps-link">Continue with the Guide<br>继续阅读该指南</p>
    <p class="next-steps-caption">The guide walks you through every aspect of the framework in full detail.<br>该指南会带你深入了解框架所有方面的细节。</p>
  </a>
  <a class="vt-box" href="/tutorial/">
    <p class="next-steps-link">Try the Tutorial<br>尝试互动教程</p>
    <p class="next-steps-caption">For those who prefer learning things hands-on.<br>适合喜欢边动手边学的读者。</p>
  </a>
  <a class="vt-box" href="/examples/">
    <p class="next-steps-link">Check out the Examples<br>查看示例</p>
    <p class="next-steps-caption">Explore examples of core features and common UI tasks.<br>浏览核心功能和常见用户界面的示例。</p>
  </a>
</div>
