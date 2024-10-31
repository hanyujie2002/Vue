---
footer: false
---

# 简介 | Introduction {#introduction}

:::info 你正在阅读的是 Vue 3 的文档！| You are reading the documentation for Vue 3!

- Vue 2 support has ended on **Dec 31, 2023**. Learn more about [Vue 2 EOL](https://v2.vuejs.org/eol/).
- Vue 2 已于 **2023 年 12 月 31 日**停止维护。详见 [Vue 2 终止支持 (EOL)](https://v2.cn.vuejs.org/eol/)。
- Upgrading from Vue 2? Check out the [Migration Guide](https://v3-migration.vuejs.org/).
- 想从 Vue 2 升级？请参考[迁移指南](https://v3-migration.vuejs.org/)。
:::

<style src="@theme/styles/vue-mastery.css"></style>
<div class="vue-mastery-link">
  <a href="https://www.vuemastery.com/courses/" target="_blank">
    <div class="banner-wrapper">
      <img class="banner" alt="Vue Mastery banner" width="96px" height="56px" src="https://storage.googleapis.com/vue-mastery.appspot.com/flamelink/media/vuemastery-graphical-link-96x56.png" />
    </div>
    <p class="description">在 <span>VueMastery</span> 上观看视频课程学习 Vue</p>
    <div class="logo-wrapper">
        <img alt="Vue Mastery Logo" width="25px" src="https://storage.googleapis.com/vue-mastery.appspot.com/flamelink/media/vue-mastery-logo.png" />
    </div>
  </a>
</div>

## 什么是 Vue？| What is Vue? {#what-is-vue}

Vue (pronounced /vjuː/, like **view**) is a JavaScript framework for building user interfaces. It builds on top of standard HTML, CSS, and JavaScript and provides a declarative, component-based programming model that helps you efficiently develop user interfaces of any complexity.

Vue (发音为 /vjuː/，类似 **view**) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。

Here is a minimal example:

下面是一个最基本的示例：

<div class="options-api">

```js
import { createApp } from 'vue'

createApp({
  data() {
    return {
      count: 0
    }
  }
}).mount('#app')
```

</div>
<div class="composition-api">

```js
import { createApp, ref } from 'vue'

createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')
```

</div>

```vue-html
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```

**结果展示 | Result**

<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<div class="demo">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>

The above example demonstrates the two core features of Vue:

上面的示例展示了 Vue 的两个核心功能：

- **Declarative Rendering**: Vue extends standard HTML with a template syntax that allows us to declaratively describe HTML output based on JavaScript state.
- **声明式渲染**：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系。

- **Reactivity**: Vue automatically tracks JavaScript state changes and efficiently updates the DOM when changes happen.
- **响应性**：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM。

You may already have questions - don't worry. We will cover every little detail in the rest of the documentation. For now, please read along so you can have a high-level understanding of what Vue offers.

You may already have questions - don't worry. We will cover every little detail in the rest of the documentation. For now, please read along so you can have a high-level understanding of what Vue offers.

你可能已经有了些疑问——先别急，在后续的文档中我们会详细介绍每一个细节。现在，请继续看下去，以确保你对 Vue 作为一个框架到底提供了什么有一个宏观的了解。

:::tip 预备知识 | Prerequisites
The rest of the documentation assumes basic familiarity with HTML, CSS, and JavaScript. If you are totally new to frontend development, it might not be the best idea to jump right into a framework as your first step - grasp the basics and then come back! You can check your knowledge level with these overviews for [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript), [HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML) and [CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps) if needed. Prior experience with other frameworks helps, but is not required.

文档接下来的内容会假设你对 HTML、CSS 和 JavaScript 已经基本熟悉。如果你对前端开发完全陌生，最好不要直接从一个框架开始进行入门学习——最好是掌握了基础知识再回到这里。如有需要，你可以通过这些 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)、[HTML](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML) 和 [CSS](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps) 概述来检验你的知识水平。如果之前有其他框架的经验会很有帮助，但也不是必须的。
:::

## 渐进式框架 | The Progressive Framework {#the-progressive-framework}

Vue is a framework and ecosystem that covers most of the common features needed in frontend development. But the web is extremely diverse - the things we build on the web may vary drastically in form and scale. With that in mind, Vue is designed to be flexible and incrementally adoptable. Depending on your use case, Vue can be used in different ways:

Vue 是一个框架，也是一个生态。其功能覆盖了大部分前端开发常见的需求。但 Web 世界是十分多样化的，不同的开发者在 Web 上构建的东西可能在形式和规模上会有很大的不同。考虑到这一点，Vue 的设计非常注重灵活性和“可以被逐步集成”这个特点。根据你的需求场景，你可以用不同的方式使用 Vue：

- Enhancing static HTML without a build step
- 无需构建步骤，渐进式增强静态的 HTML
- Embedding as Web Components on any page
- 在任何页面中作为 Web Components 嵌入
- Single-Page Application (SPA)
- 单页应用 (SPA)
- Fullstack / Server-Side Rendering (SSR)
- 全栈 / 服务端渲染 (SSR)
- Jamstack / Static Site Generation (SSG)
- Jamstack / 静态站点生成 (SSG)
- Targeting desktop, mobile, WebGL, and even the terminal
- 开发桌面端、移动端、WebGL，甚至是命令行终端中的界面

If you find these concepts intimidating, don't worry! The tutorial and guide only require basic HTML and JavaScript knowledge, and you should be able to follow along without being an expert in any of these.

如果你是初学者，可能会觉得这些概念有些复杂。别担心！理解教程和指南的内容只需要具备基础的 HTML 和 JavaScript 知识。即使你不是这些方面的专家，也能够跟得上。

if you are an experienced developer interested in how to best integrate vue into your stack, or you are curious about what these terms mean, we discuss them in more detail in [ways of using vue](/guide/extras/ways-of-using-vue).

如果你是有经验的开发者，希望了解如何以最合适的方式在项目中引入 Vue，或者是对上述的这些概念感到好奇，我们在[使用 Vue 的多种方式](/guide/extras/ways-of-using-vue)中讨论了有关它们的更多细节。

Despite the flexibility, the core knowledge about how Vue works is shared across all these use cases. Even if you are just a beginner now, the knowledge gained along the way will stay useful as you grow to tackle more ambitious goals in the future. If you are a veteran, you can pick the optimal way to leverage Vue based on the problems you are trying to solve, while retaining the same productivity. This is why we call Vue "The Progressive Framework": it's a framework that can grow with you and adapt to your needs.

无论再怎么灵活，Vue 的核心知识在所有这些用例中都是通用的。即使你现在只是一个初学者，随着你的不断成长，到未来有能力实现更复杂的项目时，这一路上获得的知识依然会适用。如果你已经是一个老手，你可以根据实际场景来选择使用 Vue 的最佳方式，在各种场景下都可以保持同样的开发效率。这就是为什么我们将 Vue 称为“渐进式框架”：它是一个可以与你共同成长、适应你不同需求的框架。

## 单文件组件 | Single-File Components {#single-file-components}

In most build-tool-enabled Vue projects, we author Vue components using an HTML-like file format called **Single-File Component** (also known as `*.vue` files, abbreviated as **SFC**). A Vue SFC, as the name suggests, encapsulates the component's logic (JavaScript), template (HTML), and styles (CSS) in a single file. Here's the previous example, written in SFC format:

在大多数启用了构建工具的 Vue 项目中，我们可以使用一种类似 HTML 格式的文件来书写 Vue 组件，它被称为**单文件组件** (也被称为 `*.vue` 文件，英文 Single-File Components，缩写为 **SFC**)。顾名思义，Vue 的单文件组件会将一个组件的逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里。下面我们将用单文件组件的格式重写上面的计数器示例：

<div class="options-api">

```vue
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

</div>
<div class="composition-api">

```vue
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

</div>

SFC is a defining feature of Vue and is the recommended way to author Vue components **if** your use case warrants a build setup. You can learn more about the [how and why of SFC](/guide/scaling-up/sfc) in its dedicated section - but for now, just know that Vue will handle all the build tools setup for you.

单文件组件是 Vue 的标志性功能。如果你的用例需要进行构建，我们推荐用它来编写 Vue 组件。你可以在后续相关章节里了解更多关于[单文件组件的用法及用途](/guide/scaling-up/sfc)。但你暂时只需要知道 Vue 会帮忙处理所有这些构建工具的配置就好。

## API 风格 | Options API {#api-styles}

Vue components can be authored in two different API styles: **Options API** and **Composition API**.

Vue 的组件可以按两种不同的风格书写：**选项式 API** 和**组合式 API**。

### 选项式 API (Options API) | Options API {#options-api}

With Options API, we define a component's logic using an object of options such as `data`, `methods`, and `mounted`. Properties defined by options are exposed on `this` inside functions, which points to the component instance:

使用选项式 API，我们可以用包含多个选项的对象来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的属性都会暴露在函数内部的 `this` 上，它会指向当前的组件实例。

```vue
<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  // data() 返回的属性将会成为响应式的状态
  // 并且暴露在 `this` 上
  data() {
    return {
      count: 0
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event handlers in templates.
  // methods 是一些用来更改状态与触发更新的函数
  // 它们可以在模板中作为事件处理器绑定
  methods: {
    increment() {
      this.count++
    }
  },

  // Lifecycle hooks are called at different stages
  // of a component's lifecycle.
  // This function will be called when the component is mounted.
  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  // 例如这个函数就会在组件挂载完成后被调用
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNptkMFqxCAQhl9lkB522ZL0HNKlpa/Qo4e1ZpLIGhUdl5bgu9es2eSyIMio833zO7NP56pbRNawNkivHJ25wV9nPUGHvYiaYOYGoK7Bo5CkbgiBBOFy2AkSh2N5APmeojePCkDaaKiBt1KnZUuv3Ky0PppMsyYAjYJgigu0oEGYDsirYUAP0WULhqVrQhptF5qHQhnpcUJD+wyQaSpUd/Xp9NysVY/yT2qE0dprIS/vsds5Mg9mNVbaDofL94jZpUgJXUKBCvAy76ZUXY53CTd5tfX2k7kgnJzOCXIF0P5EImvgQ2olr++cbRE4O3+t6JxvXj0ptXVpye1tvbFY+ge/NJZt)

### 组合式 API (Composition API) | Composition API {#composition-api}


With Composition API, we define a component's logic using imported API functions. In SFCs, Composition API is typically used with [`<script setup>`](/api/sfc-script-setup). The `setup` attribute is a hint that makes Vue perform compile-time transforms that allow us to use Composition API with less boilerplate. For example, imports and top-level variables / functions declared in `<script setup>` are directly usable in the template.

通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [`<script setup>`](/api/sfc-script-setup) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`<script setup>` 中的导入和顶层变量/函数都能够在模板中直接使用。

Here is the same component, with the exact same template, but using Composition API and `<script setup>` instead:

下面是使用了组合式 API 与 `<script setup>` 改造后和上面的模板完全一样的组件：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// reactive state
// 响应式状态
const count = ref(0)

// functions that mutate state and trigger updates
// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// lifecycle hooks
// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNpNkMFqwzAQRH9lMYU4pNg9Bye09NxbjzrEVda2iLwS0spQjP69a+yYHnRYad7MaOfiw/tqSliciybqYDxDRE7+qsiM3gWGGQJ2r+DoyyVivEOGLrgRDkIdFCmqa1G0ms2EELllVKQdRQa9AHBZ+PLtuEm7RCKVd+ChZRjTQqwctHQHDqbvMUDyd7mKip4AGNIBRyQujzArgtW/mlqb8HRSlLcEazrUv9oiDM49xGGvXgp5uT5his5iZV1f3r4HFHvDprVbaxPhZf4XkKub/CDLaep1T7IhGRhHb6WoTADNT2KWpu/aGv24qGKvrIrr5+Z7hnneQnJu6hURvKl3ryL/ARrVkuI=)

### 该选哪一个？| Which to Choose? {#which-to-choose}

Both API styles are fully capable of covering common use cases. They are different interfaces powered by the exact same underlying system. In fact, the Options API is implemented on top of the Composition API! The fundamental concepts and knowledge about Vue are shared across the two styles.

两种 API 风格都能够覆盖大部分的应用场景。它们只是同一个底层系统所提供的两套不同的接口。实际上，选项式 API 是在组合式 API 的基础上实现的！关于 Vue 的基础概念和知识在它们之间都是通用的。

The Options API is centered around the concept of a "component instance" (`this` as seen in the example), which typically aligns better with a class-based mental model for users coming from OOP language backgrounds. It is also more beginner-friendly by abstracting away the reactivity details and enforcing code organization via option groups.

选项式 API 以“组件实例”的概念为中心 (即上述例子中的 `this`)，对于有面向对象语言背景的用户来说，这通常与基于类的心智模型更为一致。同时，它将响应性相关的细节抽象出来，并强制按照选项来组织代码，从而对初学者而言更为友好。

The Composition API is centered around declaring reactive state variables directly in a function scope and composing state from multiple functions together to handle complexity. It is more free-form and requires an understanding of how reactivity works in Vue to be used effectively. In return, its flexibility enables more powerful patterns for organizing and reusing logic.

组合式 API 的核心思想是直接在函数作用域内定义响应式状态变量，并将从多个函数中得到的状态组合起来处理复杂问题。这种形式更加自由，也需要你对 Vue 的响应式系统有更深的理解才能高效使用。相应的，它的灵活性也使得组织和重用逻辑的模式变得更加强大。

You can learn more about the comparison between the two styles and the potential benefits of Composition API in the [Composition API FAQ](/guide/extras/composition-api-faq).

在[组合式 API FAQ](/guide/extras/composition-api-faq) 章节中，你可以了解更多关于这两种 API 风格的对比以及组合式 API 所带来的潜在收益。

If you are new to Vue, here's our general recommendation:

如果你是使用 Vue 的新手，这里是我们的大致建议：

- For learning purposes, go with the style that looks easier to understand to you. Again, most of the core concepts are shared between the two styles. You can always pick up the other style later.
- 在学习的过程中，推荐采用更易于自己理解的风格。再强调一下，大部分的核心概念在这两种风格之间都是通用的。熟悉了一种风格以后，你也能够很快地理解另一种风格。

- For production use:
- 在生产项目中：

  - Go with Options API if you are not using build tools, or plan to use Vue primarily in low-complexity scenarios, e.g. progressive enhancement.
  - 当你不需要使用构建工具，或者打算主要在低复杂度的场景中使用 Vue，例如渐进增强的应用场景，推荐采用选项式 API。

  - Go with Composition API + Single-File Components if you plan to build full applications with Vue.
  - 当你打算用 Vue 构建完整的单页应用，推荐采用组合式 API + 单文件组件。

You don't have to commit to only one style during the learning phase. The rest of the documentation will provide code samples in both styles where applicable, and you can toggle between them at any time using the **API Preference switches** at the top of the left sidebar.

在学习阶段，你不必只固守一种风格。在接下来的文档中我们会为你提供一系列两种风格的代码供你参考，你可以随时通过左上角的 **API 风格偏好**来做切换。

## 还有其他问题？| Still Got Questions? {#still-got-questions}

Check out our [FAQ](/about/faq).

请查看我们的 [FAQ](/about/faq)。

## 选择你的学习路径 | Pick Your Learning Path {#pick-your-learning-path}

Different developers have different learning styles. Feel free to pick a learning path that suits your preference - although we do recommend going over all of the content, if possible!

不同的开发者有不同的学习方式。尽管在可能的情况下，我们推荐你通读所有内容，但你还是可以自由地选择一种自己喜欢的学习路径！

<div class="vt-box-container next-steps">
  <a class="vt-box" href="/tutorial/">
    <p class="next-steps-link">Try the Tutorial<br>尝试互动教程</p>
    <p class="next-steps-caption">For those who prefer learning things hands-on.<br>适合喜欢边动手边学的读者。</p>
  </a>
  <a class="vt-box" href="/guide/quick-start.html">
    <p class="next-steps-link">Read the Guide<br>继续阅读该指南</p>
    <p class="next-steps-caption">The guide walks you through every aspect of the framework in full detail.<br>该指南会带你深入了解框架所有方面的细节。</p>
  </a>
  <a class="vt-box" href="/examples/">
    <p class="next-steps-link">Check out the Examples<br>查看示例</p>
    <p class="next-steps-caption">Explore examples of core features and common UI tasks.<br>浏览核心功能和常见用户界面的示例。</p>
  </a>
</div>
