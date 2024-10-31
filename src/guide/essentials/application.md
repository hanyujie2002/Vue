# 创建一个 Vue 应用 | Creating a Vue Application {#creating-a-vue-application}

## 应用实例 | The application instance {#the-application-instance}

Every Vue application starts by creating a new **application instance** with the [`createApp`](/api/application#createapp) function:

每个 Vue 应用都是通过 [`createApp`](/api/application#createapp) 函数创建一个新的 **应用实例**：

```js
import { createApp } from 'vue'

const app = createApp({
  /* root component options */
  /* 根组件选项 */
})
```

## 根组件 | The Root Component {#the-root-component}

The object we are passing into `createApp` is in fact a component. Every app requires a "root component" that can contain other components as its children.

我们传入 `createApp` 的对象实际上是一个组件，每个应用都需要一个“根组件”，其他组件将作为其子组件。

If you are using Single-File Components, we typically import the root component from another file:

如果你使用的是单文件组件，我们可以直接从另一个文件中导入根组件。

```js
import { createApp } from 'vue'
// import the root component App from a single-file component.
// 从一个单文件组件中导入根组件
import App from './App.vue'

const app = createApp(App)
```

While many examples in this guide only need a single component, most real applications are organized into a tree of nested, reusable components. For example, a Todo application's component tree might look like this:

虽然本指南中的许多示例只需要一个组件，但大多数真实的应用都是由一棵嵌套的、可重用的组件树组成的。例如，一个待办事项 (Todos) 应用的组件树可能是这样的：

```
App (root component)
├─ TodoList
│  └─ TodoItem
│     ├─ TodoDeleteButton
│     └─ TodoEditButton
└─ TodoFooter
   ├─ TodoClearButton
   └─ TodoStatistics
```

In later sections of the guide, we will discuss how to define and compose multiple components together. Before that, we will focus on what happens inside a single component.

我们会在指南的后续章节中讨论如何定义和组合多个组件。在那之前，我们得先关注一个组件内到底发生了什么。

## 挂载应用 | Mounting the App {#mounting-the-app}

An application instance won't render anything until its `.mount()` method is called. It expects a "container" argument, which can either be an actual DOM element or a selector string:

应用实例必须在调用了 `.mount()` 方法后才会渲染出来。该方法接收一个“容器”参数，可以是一个实际的 DOM 元素或是一个 CSS 选择器字符串：

```html
<div id="app"></div>
```

```js
app.mount('#app')
```

The content of the app's root component will be rendered inside the container element. The container element itself is not considered part of the app.

应用根组件的内容将会被渲染在容器元素里面。容器元素自己将**不会**被视为应用的一部分。

The `.mount()` method should always be called after all app configurations and asset registrations are done. Also note that its return value, unlike the asset registration methods, is the root component instance instead of the application instance.

`.mount()` 方法应该始终在整个应用配置和资源注册完成后被调用。同时请注意，不同于其他资源注册方法，它的返回值是根组件实例而非应用实例。

### DOM 中的根组件模板 | In-DOM Root Component Template {#in-dom-root-component-template}

The template for the root component is usually part of the component itself, but it is also possible to provide the template separately by writing it directly inside the mount container:

根组件的模板通常是组件本身的一部分，但也可以直接通过在挂载容器内编写模板来单独提供：

```html
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
```

```js
import { createApp } from 'vue'

const app = createApp({
  data() {
    return {
      count: 0
    }
  }
})

app.mount('#app')
```

Vue will automatically use the container's `innerHTML` as the template if the root component does not already have a `template` option.

当根组件没有设置 `template` 选项时，Vue 将自动使用容器的 `innerHTML` 作为模板。

In-DOM templates are often used in applications that are [using Vue without a build step](/guide/quick-start.html#using-vue-from-cdn). They can also be used in conjunction with server-side frameworks, where the root template might be generated dynamically by the server.

DOM 内模板通常用于[无构建步骤](/guide/quick-start.html#using-vue-from-cdn)的 Vue 应用程序。它们也可以与服务器端框架一起使用，其中根模板可能是由服务器动态生成的。

## 应用配置 | App Configurations {#app-configurations}

The application instance exposes a `.config` object that allows us to configure a few app-level options, for example, defining an app-level error handler that captures errors from all descendant components:

应用实例会暴露一个 `.config` 对象允许我们配置一些应用级的选项，例如定义一个应用级的错误处理器，用来捕获所有子组件上的错误：

```js
app.config.errorHandler = (err) => {
  /* handle error */
  /* 处理错误 */
}
```

The application instance also provides a few methods for registering app-scoped assets. For example, registering a component:

应用实例还提供了一些方法来注册应用范围内可用的资源，例如注册一个组件：

```js
app.component('TodoDeleteButton', TodoDeleteButton)
```

This makes the `TodoDeleteButton` available for use anywhere in our app. We will discuss registration for components and other types of assets in later sections of the guide. You can also browse the full list of application instance APIs in its [API reference](/api/application).

这使得 `TodoDeleteButton` 在应用的任何地方都是可用的。我们会在指南的后续章节中讨论关于组件和其他资源的注册。你也可以在 [API 参考](/api/application)中浏览应用实例 API 的完整列表。

Make sure to apply all app configurations before mounting the app!

确保在挂载应用实例之前完成所有应用配置！

## 多个应用实例 | Multiple application instances {#multiple-application-instances}

You are not limited to a single application instance on the same page. The `createApp` API allows multiple Vue applications to co-exist on the same page, each with its own scope for configuration and global assets:

应用实例并不只限于一个。`createApp` API 允许你在同一个页面中创建多个共存的 Vue 应用，而且每个应用都拥有自己的用于配置和全局资源的作用域。

```js
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```

If you are using Vue to enhance server-rendered HTML and only need Vue to control specific parts of a large page, avoid mounting a single Vue application instance on the entire page. Instead, create multiple small application instances and mount them on the elements they are responsible for.

如果你正在使用 Vue 来增强服务端渲染 HTML，并且只想要 Vue 去控制一个大型页面中特殊的一小部分，应避免将一个单独的 Vue 应用实例挂载到整个页面上，而是应该创建多个小的应用实例，将它们分别挂载到所需的元素上去。

<!-- zhlint disabled -->
