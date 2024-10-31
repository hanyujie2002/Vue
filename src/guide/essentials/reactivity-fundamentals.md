---
outline: deep
---

# 响应式基础 | Reactivity Fundamentals {#reactivity-fundamentals}

:::tip API 参考 | API Preference
This page and many other chapters later in the guide contain different content for the Options API and the Composition API. Your current preference is <span class="options-api">Options API</span><span class="composition-api">Composition API</span>. You can toggle between the API styles using the "API Preference" switches at the top of the left sidebar.

本页和后面很多页面中都分别包含了选项式 API 和组合式 API 的示例代码。现在你选择的是 <span class="options-api">选项式 API</span><span class="composition-api">组合式 API</span>。你可以使用左侧侧边栏顶部的“API 风格偏好”开关在 API 风格之间切换。
:::

<div class="options-api">

## 声明响应式状态 | Declaring Reactive State \* {#declaring-reactive-state}

With the Options API, we use the `data` option to declare reactive state of a component. The option value should be a function that returns an object. Vue will call the function when creating a new component instance, and wrap the returned object in its reactivity system. Any top-level properties of this object are proxied on the component instance (`this` in methods and lifecycle hooks):

选用选项式 API 时，会用 `data` 选项来声明组件的响应式状态。此选项的值应为返回一个对象的函数。Vue 将在创建新组件实例的时候调用此函数，并将函数返回的对象用响应式系统进行包装。此对象的所有顶层属性都会被代理到组件实例 (即方法和生命周期钩子中的 `this`) 上。

```js{2-6}
export default {
  data() {
    return {
      count: 1
    }
  },

  // `mounted` is a lifecycle hook which we will explain later
  // `mounted` 是生命周期钩子，之后我们会讲到
  mounted() {
    // `this` refers to the component instance.
    // `this` 指向当前组件实例
    console.log(this.count) // => 1

    // data can be mutated as well
    // 数据属性也可以被更改
    this.count = 2
  }
}
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNpFUNFqhDAQ/JXBpzsoHu2j3B2U/oYPpnGtoetGkrW2iP/eRFsPApthd2Zndilex7H8mqioimu0wY16r4W+Rx8ULXVmYsVSC9AaNafz/gcC6RTkHwHWT6IVnne85rI+1ZLr5YJmyG1qG7gIA3Yd2R/LhN77T8y9sz1mwuyYkXazcQI2SiHz/7iP3VlQexeb5KKjEKEe2lPyMIxeSBROohqxVO4E6yV6ppL9xykTy83tOQvd7tnzoZtDwhrBO2GYNFloYWLyxrzPPOi44WWLWUt618txvASUhhRCKSHgbZt2scKy7HfCujGOqWL9BVfOgyI=)

These instance properties are only added when the instance is first created, so you need to ensure they are all present in the object returned by the `data` function. Where necessary, use `null`, `undefined` or some other placeholder value for properties where the desired value isn't yet available.

这些实例上的属性仅在实例首次创建时被添加，因此你需要确保它们都出现在 `data` 函数返回的对象上。若所需的值还未准备好，在必要时也可以使用 `null`、`undefined` 或者其他一些值占位。

It is possible to add a new property directly to `this` without including it in `data`. However, properties added this way will not be able to trigger reactive updates.

虽然也可以不在 `data` 上定义，直接向组件实例添加新属性，但这个属性将无法触发响应式更新。

Vue uses a `$` prefix when exposing its own built-in APIs via the component instance. It also reserves the prefix `_` for internal properties. You should avoid using names for top-level `data` properties that start with either of these characters.

Vue 在组件实例上暴露的内置 API 使用 `$` 作为前缀。它同时也为内部属性保留 `_` 前缀。因此，你应该避免在顶层 `data` 上使用任何以这些字符作前缀的属性。

### 响应式代理 vs. 原始值 | Reactive Proxy vs. Original \* {#reactive-proxy-vs-original}

In Vue 3, data is made reactive by leveraging [JavaScript Proxies](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy). Users coming from Vue 2 should be aware of the following edge case:

在 Vue 3 中，数据是基于 [JavaScript Proxy (代理)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 实现响应式的。使用过 Vue 2 的用户可能需要注意下面这样的边界情况：

```js
export default {
  data() {
    return {
      someObject: {}
    }
  },
  mounted() {
    const newObject = {}
    this.someObject = newObject

    console.log(newObject === this.someObject) // false
  }
}
```

When you access `this.someObject` after assigning it, the value is a reactive proxy of the original `newObject`. **Unlike in Vue 2, the original `newObject` is left intact and will not be made reactive: make sure to always access reactive state as a property of `this`.**

当你在赋值后再访问 `this.someObject`，此值已经是原来的 `newObject` 的一个响应式代理。**与 Vue 2 不同的是，这里原始的 `newObject` 不会变为响应式：请确保始终通过 `this` 来访问响应式状态。**

</div>

<div class="composition-api">

## 声明响应式状态 | Declaring Reactive State \*\* {#declaring-reactive-state-1}

### `ref()` \*\* {#ref}

In Composition API, the recommended way to declare reactive state is using the [`ref()`](/api/reactivity-core#ref) function:

在组合式 API 中，推荐使用 [`ref()`](/api/reactivity-core#ref) 函数来声明响应式状态：

```js
import { ref } from 'vue'

const count = ref(0)
```

`ref()` takes the argument and returns it wrapped within a ref object with a `.value` property:

`ref()` 接收参数，并将其包裹在一个带有 `.value` 属性的 ref 对象中返回：

```js
const count = ref(0)

console.log(count) // { value: 0 }
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

> See also: [Typing Refs](/guide/typescript/composition-api#typing-ref) <sup class="vt-badge ts" />
>
> 参考：[为 refs 标注类型](/guide/typescript/composition-api#typing-ref) <sup class="vt-badge ts" />

To access refs in a component's template, declare and return them from a component's `setup()` function:

要在组件模板中访问 ref，请从组件的 `setup()` 函数中声明并返回它们：

```js{5,9-11}
import { ref } from 'vue'

export default {
  // `setup` is a special hook dedicated for the Composition API.
  // `setup` 是一个特殊的钩子，专门用于组合式 API。
  setup() {
    const count = ref(0)

    // expose the ref to the template
    // 将 ref 暴露给模板
    return {
      count
    }
  }
}
```

```vue-html
<div>{{ count }}</div>
```

Notice that we did **not** need to append `.value` when using the ref in the template. For convenience, refs are automatically unwrapped when used inside templates (with a few [caveats](#caveat-when-unwrapping-in-templates)).

注意，在模板中使用 ref 时，我们**不**需要附加 `.value`。为了方便起见，当在模板中使用时，ref 会自动解包 (有一些[注意事项](#caveat-when-unwrapping-in-templates))。

You can also mutate a ref directly in event handlers:

你也可以直接在事件监听器中改变一个 ref：

```vue-html{1}
<button @click="count++">
  {{ count }}
</button>
```

For more complex logic, we can declare functions that mutate refs in the same scope and expose them as methods alongside the state:

对于更复杂的逻辑，我们可以在同一作用域内声明更改 ref 的函数，并将它们作为方法与状态一起公开：

```js{7-10,15}
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    function increment() {
      // .value is needed in JavaScript
      // 在 JavaScript 中需要 .value
      count.value++
    }

    // don't forget to expose the function as well.
    // 不要忘记同时暴露 increment 函数
    return {
      count,
      increment
    }
  }
}
```

Exposed methods can then be used as event handlers:

然后，暴露的方法可以被用作事件监听器：

```vue-html{1}
<button @click="increment">
  {{ count }}
</button>
```

Here's the example live on [Codepen](https://codepen.io/vuejs-examples/pen/WNYbaqo), without using any build tools.

这里是 [Codepen](https://codepen.io/vuejs-examples/pen/WNYbaqo) 上的例子，没有使用任何构建工具。

### `<script setup>` \*\* {#script-setup}

Manually exposing state and methods via `setup()` can be verbose. Luckily, it can be avoided when using [Single-File Components (SFCs)](/guide/scaling-up/sfc). We can simplify the usage with `<script setup>`:

在 `setup()` 函数中手动暴露大量的状态和方法非常繁琐。幸运的是，我们可以通过使用[单文件组件 (SFC)](/guide/scaling-up/sfc) 来避免这种情况。我们可以使用 `<script setup>` 来大幅度地简化代码：

```vue{1}
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNo9jUEKgzAQRa8yZKMiaNcllvYe2dgwQqiZhDhxE3L3jrW4/DPvv1/UK8Zhz6juSm82uciwIef4MOR8DImhQMIFKiwpeGgEbQwZsoE2BhsyMUwH0d66475ksuwCgSOb0CNx20ExBCc77POase8NVUN6PBdlSwKjj+vMKAlAvzOzWJ52dfYzGXXpjPoBAKX856uopDGeFfnq8XKp+gWq4FAi)

Top-level imports, variables and functions declared in `<script setup>` are automatically usable in the template of the same component. Think of the template as a JavaScript function declared in the same scope - it naturally has access to everything declared alongside it.

`<script setup>` 中的顶层的导入、声明的变量和函数可在同一组件的模板中直接使用。你可以理解为模板是在同一作用域内声明的一个 JavaScript 函数——它自然可以访问与它一起声明的所有内容。

:::tip
For the rest of the guide, we will be primarily using SFC + `<script setup>` syntax for the Composition API code examples, as that is the most common usage for Vue developers.

在指南的后续章节中，我们基本上都会在组合式 API 示例中使用单文件组件 + `<script setup>` 的语法，因为大多数 Vue 开发者都会这样使用。

If you are not using SFC, you can still use Composition API with the [`setup()`](/api/composition-api-setup) option.

如果你没有使用单文件组件，你仍然可以在 [`setup()`](/api/composition-api-setup) 选项中使用组合式 API。
:::

### 为什么要使用 ref？| Why Refs?  \*\* {#why-refs}

You might be wondering why we need refs with the `.value` instead of plain variables. To explain that, we will need to briefly discuss how Vue's reactivity system works.

你可能会好奇：为什么我们需要使用带有 `.value` 的 ref，而不是普通的变量？为了解释这一点，我们需要简单地讨论一下 Vue 的响应式系统是如何工作的。

When you use a ref in a template, and change the ref's value later, Vue automatically detects the change and updates the DOM accordingly. This is made possible with a dependency-tracking based reactivity system. When a component is rendered for the first time, Vue **tracks** every ref that was used during the render. Later on, when a ref is mutated, it will **trigger** a re-render for components that are tracking it.

当你在模板中使用了一个 ref，然后改变了这个 ref 的值时，Vue 会自动检测到这个变化，并且相应地更新 DOM。这是通过一个基于依赖追踪的响应式系统实现的。当一个组件首次渲染时，Vue 会**追踪**在渲染过程中使用的每一个 ref。然后，当一个 ref 被修改时，它会**触发**追踪它的组件的一次重新渲染。

In standard JavaScript, there is no way to detect the access or mutation of plain variables. However, we can intercept the get and set operations of an object's properties using getter and setter methods.

在标准的 JavaScript 中，检测普通变量的访问或修改是行不通的。然而，我们可以通过 getter 和 setter 方法来拦截对象属性的 get 和 set 操作。

The `.value` property gives Vue the opportunity to detect when a ref has been accessed or mutated. Under the hood, Vue performs the tracking in its getter, and performs triggering in its setter. Conceptually, you can think of a ref as an object that looks like this:

该 `.value` 属性给予了 Vue 一个机会来检测 ref 何时被访问或修改。在其内部，Vue 在它的 getter 中执行追踪，在它的 setter 中执行触发。从概念上讲，你可以将 ref 看作是一个像这样的对象：

```js
// pseudo code, not actual implementation
// 伪代码，不是真正的实现
const myRef = {
  _value: 0,
  get value() {
    track()
    return this._value
  },
  set value(newValue) {
    this._value = newValue
    trigger()
  }
}
```

Another nice trait of refs is that unlike plain variables, you can pass refs into functions while retaining access to the latest value and the reactivity connection. This is particularly useful when refactoring complex logic into reusable code.

另一个 ref 的好处是，与普通变量不同，你可以将 ref 传递给函数，同时保留对最新值和响应式连接的访问。当将复杂的逻辑重构为可重用的代码时，这将非常有用。

The reactivity system is discussed in more details in the [Reactivity in Depth](/guide/extras/reactivity-in-depth) section.

该响应性系统在[深入响应式原理](/guide/extras/reactivity-in-depth)章节中有更详细的讨论。
</div>

<div class="options-api">

## 声明方法 | Declaring Methods \* {#declaring-methods}

<VueSchoolLink href="https://vueschool.io/lessons/methods-in-vue-3" title="免费的 Vue.js Methods 课程 | Free Vue.js Methods Lesson"/>

To add methods to a component instance we use the `methods` option. This should be an object containing the desired methods:

要为组件添加方法，我们需要用到 `methods` 选项。它应该是一个包含所有方法的对象：

```js{7-11}
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  mounted() {
    // methods can be called in lifecycle hooks, or other methods!
    // 在其他方法或是生命周期中也可以调用方法
    this.increment()
  }
}
```

Vue automatically binds the `this` value for `methods` so that it always refers to the component instance. This ensures that a method retains the correct `this` value if it's used as an event listener or callback. You should avoid using arrow functions when defining `methods`, as that prevents Vue from binding the appropriate `this` value:

Vue 自动为 `methods` 中的方法绑定了永远指向组件实例的 `this`。这确保了方法在作为事件监听器或回调函数时始终保持正确的 `this`。你不应该在定义 `methods` 时使用箭头函数，因为箭头函数没有自己的 `this` 上下文。

```js
export default {
  methods: {
    increment: () => {
      // BAD: no `this` access here!
      // 反例：无法访问此处的 `this`!
    }
  }
}
```

Just like all other properties of the component instance, the `methods` are accessible from within the component's template. Inside a template they are most commonly used as event listeners:

和组件实例上的其他属性一样，方法也可以在模板上被访问。在模板中它们常常被用作事件监听器：

```vue-html
<button @click="increment">{{ count }}</button>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNplj9EKwyAMRX8l+LSx0e65uLL9hy+dZlTWqtg4BuK/z1baDgZicsPJgUR2d656B2QN45P02lErDH6c9QQKn10YCKIwAKqj7nAsPYBHCt6sCUDaYKiBS8lpLuk8/yNSb9XUrKg20uOIhnYXAPV6qhbF6fRvmOeodn6hfzwLKkx+vN5OyIFwdENHmBMAfwQia+AmBy1fV8E2gWBtjOUASInXBcxLvN4MLH0BCe1i4Q==)

In the example above, the method `increment` will be called when the `<button>` is clicked.

在上面的例子中，`increment` 方法会在 `<button>` 被点击时调用。

</div>

### 深层响应性 | Deep Reactivity {#deep-reactivity}

<div class="options-api">

In Vue, state is deeply reactive by default. This means you can expect changes to be detected even when you mutate nested objects or arrays:

在 Vue 中，默认情况下，状态是深度响应的。这意味着当改变嵌套对象或数组时，这些变化也会被检测到：

```js
export default {
  data() {
    return {
      obj: {
        nested: { count: 0 },
        arr: ['foo', 'bar']
      }
    }
  },
  methods: {
    mutateDeeply() {
      // these will work as expected.
      // 以下都会按照期望工作
      this.obj.nested.count++
      this.obj.arr.push('baz')
    }
  }
}
```

</div>

<div class="composition-api">

Refs can hold any value type, including deeply nested objects, arrays, or JavaScript built-in data structures like `Map`.

Ref 可以持有任何类型的值，包括深层嵌套的对象、数组或者 JavaScript 内置的数据结构，比如 `Map`。

A ref will make its value deeply reactive. This means you can expect changes to be detected even when you mutate nested objects or arrays:

Ref 会使它的值具有深层响应性。这意味着即使改变嵌套对象或数组时，变化也会被检测到：

```js
import { ref } from 'vue'

const obj = ref({
  nested: { count: 0 },
  arr: ['foo', 'bar']
})

function mutateDeeply() {
  // these will work as expected.
  // 以下都会按照期望工作
  obj.value.nested.count++
  obj.value.arr.push('baz')
}
```

Non-primitive values are turned into reactive proxies via [`reactive()`](#reactive), which is discussed below.

非原始值将通过 [`reactive()`](#reactive) 转换为响应式代理，该函数将在后面讨论。

It is also possible to opt-out of deep reactivity with [shallow refs](/api/reactivity-advanced#shallowref). For shallow refs, only `.value` access is tracked for reactivity. Shallow refs can be used for optimizing performance by avoiding the observation cost of large objects, or in cases where the inner state is managed by an external library.

也可以通过 [shallow ref](/api/reactivity-advanced#shallowref) 来放弃深层响应性。对于浅层 ref，只有 `.value` 的访问会被追踪。浅层 ref 可以用于避免对大型数据的响应性开销来优化性能、或者有外部库管理其内部状态的情况。

Further reading:

阅读更多：

- [减少大型不可变数据的响应性开销 | Reduce Reactivity Overhead for Large Immutable Structures](/guide/best-practices/performance#reduce-reactivity-overhead-for-large-immutable-structures)
- [与外部状态系统集成 | Integration with External State Systems](/guide/extras/reactivity-in-depth#integration-with-external-state-systems)

</div>

### DOM 更新时机 | DOM Update Timing {#dom-update-timing}

When you mutate reactive state, the DOM is updated automatically. However, it should be noted that the DOM updates are not applied synchronously. Instead, Vue buffers them until the "next tick" in the update cycle to ensure that each component updates only once no matter how many state changes you have made.

当你修改了响应式状态时，DOM 会被自动更新。但是需要注意的是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。

To wait for the DOM update to complete after a state change, you can use the [nextTick()](/api/general#nexttick) global API:

要等待 DOM 更新完成后再执行额外的代码，可以使用 [nextTick()](/api/general#nexttick) 全局 API：

<div class="composition-api">

```js
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // Now the DOM is updated
  // 现在 DOM 已经更新了
}
```

</div>
<div class="options-api">

```js
import { nextTick } from 'vue'

export default {
  methods: {
    async increment() {
      this.count++
      await nextTick()
      // Now the DOM is updated
      // 现在 DOM 已经更新了
    }
  }
}
```

</div>

<div class="composition-api">

## `reactive()` \*\* {#reactive}

There is another way to declare reactive state, with the `reactive()` API. Unlike a ref which wraps the inner value in a special object, `reactive()` makes an object itself reactive:

还有另一种声明响应式状态的方式，即使用 `reactive()` API。与将内部值包装在特殊对象中的 ref 不同，`reactive()` 将使对象本身具有响应性：

```js
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

> See also: [Typing Reactive](/guide/typescript/composition-api#typing-reactive) <sup class="vt-badge ts" />
>
> 参考：[为 `reactive()` 标注类型](/guide/typescript/composition-api#typing-reactive) <sup class="vt-badge ts" />

Usage in template:

在模板中使用：

```vue-html
<button @click="state.count++">
  {{ state.count }}
</button>
```

Reactive objects are [JavaScript Proxies](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) and behave just like normal objects. The difference is that Vue is able to intercept the access and mutation of all properties of a reactive object for reactivity tracking and triggering.

响应式对象是 [JavaScript 代理](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为就和普通对象一样。不同的是，Vue 能够拦截对响应式对象所有属性的访问和修改，以便进行依赖追踪和触发更新。

`reactive()` converts the object deeply: nested objects are also wrapped with `reactive()` when accessed. It is also called by `ref()` internally when the ref value is an object. Similar to shallow refs, there is also the [`shallowReactive()`](/api/reactivity-advanced#shallowreactive) API for opting-out of deep reactivity.

`reactive()` 将深层地转换对象：当访问嵌套对象时，它们也会被 `reactive()` 包装。当 ref 的值是一个对象时，`ref()` 也会在内部调用它。与浅层 ref 类似，这里也有一个 [`shallowReactive()`](/api/reactivity-advanced#shallowreactive) API 可以选择退出深层响应性。

### Reactive Proxy vs. Original \*\* {#reactive-proxy-vs-original-1}

It is important to note that the returned value from `reactive()` is a [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) of the original object, which is not equal to the original object:

值得注意的是，`reactive()` 返回的是一个原始对象的 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，它和原始对象是不相等的：

```js
const raw = {}
const proxy = reactive(raw)

// proxy is NOT equal to the original.
// 代理对象和原始对象不是全等的
console.log(proxy === raw) // false
```

Only the proxy is reactive - mutating the original object will not trigger updates. Therefore, the best practice when working with Vue's reactivity system is to **exclusively use the proxied versions of your state**.

只有代理对象是响应式的，更改原始对象不会触发更新。因此，使用 Vue 的响应式系统的最佳实践是**仅使用你声明对象的代理版本**。

To ensure consistent access to the proxy, calling `reactive()` on the same object always returns the same proxy, and calling `reactive()` on an existing proxy also returns that same proxy:

为保证访问代理的一致性，对同一个原始对象调用 `reactive()` 会总是返回同样的代理对象，而对一个已存在的代理对象调用 `reactive()` 会返回其本身：

```js
// calling reactive() on the same object returns the same proxy
// 在同一个对象上调用 reactive() 会返回相同的代理
console.log(reactive(raw) === proxy) // true

// calling reactive() on a proxy returns itself
// 在一个代理上调用 reactive() 会返回它自己
console.log(reactive(proxy) === proxy) // true
```

This rule applies to nested objects as well. Due to deep reactivity, nested objects inside a reactive object are also proxies:

这个规则对嵌套对象也适用。依靠深层响应性，响应式对象内的嵌套对象依然是代理：

```js
const proxy = reactive({})

const raw = {}
proxy.nested = raw

console.log(proxy.nested === raw) // false
```

### `reactive()` 的局限性 | Limitations of `reactive()` \*\* {#limitations-of-reactive}

The `reactive()` API has a few limitations:

`reactive()` API 有一些局限性：

1. **Limited value types:** it only works for object types (objects, arrays, and [collection types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections) such as `Map` and `Set`). It cannot hold [primitive types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) such as `string`, `number` or `boolean`.

- **有限的值类型**：它只能用于对象类型 (对象、数组和如 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections))。它不能持有如 `string`、`number` 或 `boolean` 这样的[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)。

2. **Cannot replace entire object:** since Vue's reactivity tracking works over property access, we must always keep the same reference to the reactive object. This means we can't easily "replace" a reactive object because the reactivity connection to the first reference is lost:

- **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。这意味着我们不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失：

   ```js
   let state = reactive({ count: 0 })

   // the above reference ({ count: 0 }) is no longer being tracked
   // (reactivity connection is lost!)
   // 上面的 ({ count: 0 }) 引用将不再被追踪
   // (响应性连接已丢失！)
   state = reactive({ count: 1 })
   ```

3. **Not destructure-friendly:** when we destructure a reactive object's primitive type property into local variables, or when we pass that property into a function, we will lose the reactivity connection:
- **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

   ```js
   const state = reactive({ count: 0 })

   // count is disconnected from state.count when destructured.
   // 当解构时，count 已经与 state.count 断开连接
   let { count } = state
   // does not affect original state
   // 不会影响原始的 state
   count++

   // the function receives a plain number and
   // won't be able to track changes to state.count
   // we have to pass the entire object in to retain reactivity
   // 该函数接收到的是一个普通的数字
   // 并且无法追踪 state.count 的变化
   // 我们必须传入整个对象以保持响应性
   callSomeFunction(state.count)
   ```

Due to these limitations, we recommend using `ref()` as the primary API for declaring reactive state.

由于这些限制，我们建议使用 `ref()` 作为声明响应式状态的主要 API。

## 额外的 ref 解包细节 | Additional Ref Unwrapping Details \*\* {#additional-ref-unwrapping-details}

### 作为 reactive 对象的属性 | As Reactive Object Property \*\* {#ref-unwrapping-as-reactive-object-property}

A ref is automatically unwrapped when accessed or mutated as a property of a reactive object. In other words, it behaves like a normal property :

一个 ref 会在作为响应式对象的属性被访问或修改时自动解包。换句话说，它的行为就像一个普通的属性：

```js
const count = ref(0)
const state = reactive({
  count
})

console.log(state.count) // 0

state.count = 1
console.log(count.value) // 1
```

If a new ref is assigned to a property linked to an existing ref, it will replace the old ref:

如果将一个新的 ref 赋值给一个关联了已有 ref 的属性，那么它会替换掉旧的 ref：

```js
const otherCount = ref(2)

state.count = otherCount
console.log(state.count) // 2
// original ref is now disconnected from state.count
// 原始 ref 现在已经和 state.count 失去联系
console.log(count.value) // 1
```

Ref unwrapping only happens when nested inside a deep reactive object. It does not apply when it is accessed as a property of a [shallow reactive object](/api/reactivity-advanced#shallowreactive).

只有当嵌套在一个深层响应式对象内时，才会发生 ref 解包。当其作为[浅层响应式对象](/api/reactivity-advanced#shallowreactive)的属性被访问时不会解包。

### 数组和集合的注意事项 | Caveat in Arrays and Collections \*\* {#caveat-in-arrays-and-collections}

Unlike reactive objects, there is **no** unwrapping performed when the ref is accessed as an element of a reactive array or a native collection type like `Map`:

与 reactive 对象不同的是，当 ref 作为响应式数组或原生集合类型 (如 `Map`) 中的元素被访问时，它**不会**被解包：

```js
const books = reactive([ref('Vue 3 Guide')])
// need .value here
// 这里需要 .value
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// need .value here
// 这里需要 .value
console.log(map.get('count').value)
```

### 在模板中解包的注意事项 | Caveat when Unwrapping in Templates \*\* {#caveat-when-unwrapping-in-templates}

Ref unwrapping in templates only applies if the ref is a top-level property in the template render context.

在模板渲染上下文中，只有顶级的 ref 属性才会被解包。

In the example below, `count` and `object` are top-level properties, but `object.id` is not:

在下面的例子中，`count` 和 `object` 是顶级属性，但 `object.id` 不是：

```js
const count = ref(0)
const object = { id: ref(1) }
```

Therefore, this expression works as expected:

因此，这个表达式按预期工作：

```vue-html
{{ count + 1 }}
```

...while this one does **NOT**:

...但这个**不会**：

```vue-html
{{ object.id + 1 }}
```

The rendered result will be `[object Object]1` because `object.id` is not unwrapped when evaluating the expression and remains a ref object. To fix this, we can destructure `id` into a top-level property:

渲染的结果将是 `[object Object]1`，因为在计算表达式时 `object.id` 没有被解包，仍然是一个 ref 对象。为了解决这个问题，我们可以将 `id` 解构为一个顶级属性：

```js
const { id } = object
```

```vue-html
{{ id + 1 }}
```

Now the render result will be `2`.

现在渲染的结果将是 `2`。

Another thing to note is that a ref does get unwrapped if it is the final evaluated value of a text interpolation (i.e. a <code v-pre>{{ }}</code> tag), so the following will render `1`:

另一个需要注意的点是，如果 ref 是文本插值的最终计算值 (即 <code v-pre>{{ }}</code> 标签)，那么它将被解包，因此以下内容将渲染为 `1`：

```vue-html
{{ object.id }}
```

This is just a convenience feature of text interpolation and is equivalent to <code v-pre>{{ object.id.value }}</code>.

该特性仅仅是文本插值的一个便利特性，等价于 <code v-pre>{{ object.id.value }}</code>。

</div>

<div class="options-api">

### 有状态方法 | Stateful Methods \* {#stateful-methods}

In some cases, we may need to dynamically create a method function, for example creating a debounced event handler:

在某些情况下，我们可能需要动态地创建一个方法函数，比如创建一个预置防抖的事件处理器：

```js
import { debounce } from 'lodash-es'

export default {
  methods: {
    // Debouncing with Lodash
    // 使用 Lodash 的防抖函数
    click: debounce(function () {
      // ... respond to click ...
      // ... 对点击的响应 ...
    }, 500)
  }
}
```

However, this approach is problematic for components that are reused because a debounced function is **stateful**: it maintains some internal state on the elapsed time. If multiple component instances share the same debounced function, they will interfere with one another.

不过这种方法对于被重用的组件来说是有问题的，因为这个预置防抖的函数是**有状态的**：它在运行时维护着一个内部状态。如果多个组件实例都共享这同一个预置防抖的函数，那么它们之间将会互相影响。

To keep each component instance's debounced function independent of the others, we can create the debounced version in the `created` lifecycle hook:

要保持每个组件实例的防抖函数都彼此独立，我们可以改为在 `created` 生命周期钩子中创建这个预置防抖的函数：

```js
export default {
  created() {
    // each instance now has its own copy of debounced handler
    // 每个实例都有了自己的预置防抖的处理函数
    this.debouncedClick = _.debounce(this.click, 500)
  },
  unmounted() {
    // also a good idea to cancel the timer
    // when the component is removed
    // 最好是在组件卸载时
    // 清除掉防抖计时器
    this.debouncedClick.cancel()
  },
  methods: {
    click() {
      // ... respond to click ...
      // ... 对点击的响应 ...
    }
  }
}
```

</div>
