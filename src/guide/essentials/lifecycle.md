# 生命周期钩子 | Lifecycle Hooks {#lifecycle-hooks}

Each Vue component instance goes through a series of initialization steps when it's created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it also runs functions called lifecycle hooks, giving users the opportunity to add their own code at specific stages.

每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

## 注册周期钩子 | Registering Lifecycle Hooks {#registering-lifecycle-hooks}

For example, the <span class="composition-api">`onMounted`</span><span class="options-api">`mounted`</span> hook can be used to run code after the component has finished the initial rendering and created the DOM nodes:

举例来说，<span class="composition-api">`onMounted`</span><span class="options-api">`mounted`</span> 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码：

<div class="composition-api">

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

</div>
<div class="options-api">

```js
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```

</div>

There are also other hooks which will be called at different stages of the instance's lifecycle, with the most commonly used being <span class="composition-api">[`onMounted`](/api/composition-api-lifecycle#onmounted), [`onUpdated`](/api/composition-api-lifecycle#onupdated), and [`onUnmounted`](/api/composition-api-lifecycle#onunmounted).</span><span class="options-api">[`mounted`](/api/options-lifecycle#mounted), [`updated`](/api/options-lifecycle#updated), and [`unmounted`](/api/options-lifecycle#unmounted).</span>

还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 <span class="composition-api">[`onMounted`](/api/composition-api-lifecycle#onmounted)、[`onUpdated`](/api/composition-api-lifecycle#onupdated) 和 [`onUnmounted`](/api/composition-api-lifecycle#onunmounted)。所有生命周期钩子的完整参考及其用法请参考 [API 索引](/api/composition-api-lifecycle.html)。</span><span class="options-api">[`mounted`](/api/options-lifecycle#mounted)、[`updated`](/api/options-lifecycle#updated) 和 [`unmounted`](/api/options-lifecycle#unmounted)。</span>

<div class="options-api">

All lifecycle hooks are called with their `this` context pointing to the current active instance invoking it. Note this means you should avoid using arrow functions when declaring lifecycle hooks, as you won't be able to access the component instance via `this` if you do so.

所有生命周期钩子函数的 `this` 上下文都会自动指向当前调用它的组件实例。注意：避免用箭头函数来定义生命周期钩子，因为如果这样的话你将无法在函数中通过 `this` 获取组件实例。

</div>

<div class="composition-api">

When calling `onMounted`, Vue automatically associates the registered callback function with the current active component instance. This requires these hooks to be registered **synchronously** during component setup. For example, do not do this:

当调用 `onMounted` 时，Vue 会自动将回调函数注册到当前正被初始化的组件实例上。这意味着这些钩子应当在组件初始化时被**同步**注册。例如，请不要这样做：

```js
setTimeout(() => {
  onMounted(() => {
    // 异步注册时当前组件实例已丢失
    // 这将不会正常工作
  })
}, 100)
```

Do note this doesn't mean that the call must be placed lexically inside `setup()` or `<script setup>`. `onMounted()` can be called in an external function as long as the call stack is synchronous and originates from within `setup()`.

注意这并不意味着对 `onMounted` 的调用必须放在 `setup()` 或 `<script setup>` 内的词法上下文中。`onMounted()` 也可以在一个外部函数中调用，只要调用栈是同步的，且最终起源自 `setup()` 就可以。

</div>

## 生命周期图示 | Lifecycle Diagram {#lifecycle-diagram}

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but as you learn and build more, it will be a useful reference.

下面是实例生命周期的图表。你现在并不需要完全理解图中的所有内容，但以后它将是一个有用的参考。

![组件生命周期图示 | Component lifecycle diagram](./images/lifecycle_zh-CN.png)

<!-- https://www.figma.com/file/Xw3UeNMOralY6NV7gSjWdS/Vue-Lifecycle -->
<!-- https://www.figma.com/file/QHo4ehJ4TRx3f7gzRP1F1k/Vue-Lifecycle-(zh-CN) -->

Consult the <span class="composition-api">[Lifecycle Hooks API reference](/api/composition-api-lifecycle)</span><span class="options-api">[Lifecycle Hooks API reference](/api/options-lifecycle)</span> for details on all lifecycle hooks and their respective use cases.

有关所有生命周期钩子及其各自用例的详细信息，请参考<span class="composition-api">[生命周期钩子 API 索引](/api/composition-api-lifecycle)</span><span class="options-api">[生命周期钩子 API 索引](/api/options-lifecycle)</span>。
