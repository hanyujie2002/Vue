# 侦听器 | Watchers {#watchers}

## 基本示例 | Basic Example {#basic-example}

Computed properties allow us to declaratively compute derived values. However, there are cases where we need to perform "side effects" in reaction to state changes - for example, mutating the DOM, or changing another piece of state based on the result of an async operation.

计算属性允许我们声明性地计算衍生值。然而在有些情况下，我们需要在状态变化时执行一些“副作用”：例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。

<div class="options-api">

With the Options API, we can use the [`watch` option](/api/options-state#watch) to trigger a function whenever a reactive property changes:

在选项式 API 中，我们可以使用 [`watch` 选项](/api/options-state#watch)在每次响应式属性发生变化时触发一个函数。

```js
export default {
  data() {
    return {
      question: '',
      answer: 'Questions usually contain a question mark. ;-)',
      loading: false
    }
  },
  watch: {
    // whenever question changes, this function will run
    // 每当 question 改变时，这个函数就会执行
    question(newQuestion, oldQuestion) {
      if (newQuestion.includes('?')) {
        this.getAnswer()
      }
    }
  },
  methods: {
    async getAnswer() {
      this.loading = true
      this.answer = 'Thinking...'
      try {
        const res = await fetch('https://yesno.wtf/api')
        this.answer = (await res.json()).answer
      } catch (error) {
        this.answer = 'Error! Could not reach the API. ' + error
      } finally {
        this.loading = false
      }
    }
  }
}
```

```vue-html
<p>
  Ask a yes/no question:
  <input v-model="question" :disabled="loading" />
</p>
<p>{{ answer }}</p>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNp9VE1v2zAM/SucLnaw1D70lqUbsiKH7rB1W4++aDYdq5ElTx9xgiD/fbT8lXZFAQO2+Mgn8pH0mW2aJjl4ZCu2trkRjfucKTw22jgosOReOjhnCqDgjseL/hvAoPNGjSeAvx6tE1qtIIqWo5Er26Ih088BteCt51KeINfKcaGAT5FQc7NP4NPNYiaQmhdC7VZQcmlxMF+61yUcWu7yajVmkabQVqjwgGZmzSuudmiX4CphofQqD+ZWSAnGqz5y9I4VtmOuS9CyGA9T3QCihGu3RKhc+gJtHH2JFld+EG5Mdug2QYZ4MSKhgBd11OgqXdipEm5PKoer0Jk2kA66wB044/EF1GtOSPRUCbUnryRJosnFnK4zpC5YR7205M9bLhyUSIrGUeVcY1dpekKrdNK6MuWNiKYKXt8V98FElDxbknGxGLCpZMi7VkGMxmjzv0pz1tvO4QPcay8LULoj5RToKoTN40MCEXyEQDJTl0KFmXpNOqsUxudN+TNFzzqdJp8ODutGcod0Alg34QWwsXsaVtIjVXqe9h5bC9V4B4ebWhco7zI24hmDVSEs/yOxIPOQEFnTnjzt2emS83nYFrhcevM6nRJhS+Ys9aoUu6Av7WqoNWO5rhsh0fxownplbBqhjJEmuv0WbN2UDNtDMRXm+zfsz/bY2TL2SH1Ec8CMTZjjhqaxh7e/v+ORvieQqvaSvN8Bf6HV0veSdG5fvSoo7Su/kO1D3f13SKInuz06VHYsahzzfl0yRj+s+3dKn9O9TW7HPrPLP624lFU=)

The `watch` option also supports a dot-delimited path as the key:

`watch` 选项也支持把键设置成用 `.` 分隔的路径：

```js
export default {
  watch: {
    // Note: only simple paths. Expressions are not supported.
    // 注意：只能是简单的路径，不支持表达式。
    'some.nested.key'(newValue) {
      // ...
    }
  }
}
```

</div>

<div class="composition-api">

With Composition API, we can use the [`watch` function](/api/reactivity-core#watch) to trigger a callback whenever a piece of reactive state changes:

在组合式 API 中，我们可以使用 [`watch` 函数](/api/reactivity-core#watch)在每次响应式状态发生变化时触发回调函数：

```vue
<script setup>
import { ref, watch } from 'vue'

const question = ref('')
const answer = ref('Questions usually contain a question mark. ;-)')
const loading = ref(false)

// watch works directly on a ref
// 可以直接侦听一个 ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})
</script>

<template>
  <p>
    Ask a yes/no question:
    <input v-model="question" :disabled="loading" />
  </p>
  <p>{{ answer }}</p>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNp9U8Fy0zAQ/ZVFF9tDah96C2mZ0umhHKBAj7oIe52oUSQjyXEyGf87KytyoDC9JPa+p+e3b1cndtd15b5HtmQrV1vZeXDo++6Wa7nrjPVwAovtAgbh6w2M0Fqzg4xOZFxzXRvtPPzq0XlpNNwEbp5lRUKEdgPaVP925jnoXS+UOgKxvJAaxEVjJ+y2hA9XxUVFGdFIvT7LtEI5JIzrqjrbGozdOmikxdqTKqmIQOV6gvOkvQDhjrqGXOOQvCzAqCa9FHBzCyeuAWT7F6uUulZ9gy7PPmZFETmQjJV7oXoke972GJHY+Axkzxupt4FalhRcYHh7TDIQcqA+LTriikFIDy0G59nG+84tq+qITpty8G0lOhmSiedefSaPZ0mnfHFG50VRRkbkj1BPceVorbFzF/+6fQj4O7g3vWpAm6Ao6JzfINw9PZaQwXuYNJJuK/U0z1nxdTLT0M7s8Ec/I3WxquLS0brRi8ddp4RHegNYhR0M/Du3pXFSAJU285osI7aSuus97K92pkF1w1nCOYNlI534qbCh8tkOVasoXkV1+sjplLZ0HGN5Vc1G2IJ5R8Np5XpKlK7J1CJntdl1UqH92k0bzdkyNc8ZRWGGz1MtbMQi1esN1tv/1F/cIdQ4e6LJod0jZzPmhV2jj/DDjy94oOcZpK57Rew3wO/ojOpjJIH2qdcN2f6DN7l9nC47RfTsHg4etUtNpZUeJz5ndPPv32j9Yve6vE6DZuNvu1R2Tg==)

### 侦听数据源类型 | Watch Source Types {#watch-source-types}

`watch`'s first argument can be different types of reactive "sources": it can be a ref (including computed refs), a reactive object, a [getter function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get#description), or an array of multiple sources:

`watch` 的第一个参数可以是不同形式的“数据源”：它可以是一个 ref (包括计算属性)、一个响应式对象、一个 [getter 函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/get#description)、或多个数据源组成的数组：

```js
const x = ref(0)
const y = ref(0)

// single ref
// 单个 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter
// getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// array of multiple sources
// 多个来源组成的数组
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
```

Do note that you can't watch a property of a reactive object like this:

注意，你不能直接侦听响应式对象的属性值，例如:

```js
const obj = reactive({ count: 0 })

// 错误，因为 watch() 得到的参数是一个 number
watch(obj.count, (count) => {
  console.log(`Count is: ${count}`)
})
```

Instead, use a getter:

这里需要用一个返回该属性的 getter 函数：

```js
// instead, use a getter:
// 提供一个 getter 函数
watch(
  () => obj.count,
  (count) => {
    console.log(`Count is: ${count}`)
  }
)
```

</div>

## 深层侦听器 | Deep Watchers {#deep-watchers}

<div class="options-api">

`watch` is shallow by default: the callback will only trigger when the watched property has been assigned a new value - it won't trigger on nested property changes. If you want the callback to fire on all nested mutations, you need to use a deep watcher:

`watch` 默认是浅层的：被侦听的属性，仅在被赋新值时，才会触发回调函数——而嵌套属性的变化不会触发。如果想侦听所有嵌套的变更，你需要深层侦听器：

```js
export default {
  watch: {
    someObject: {
      handler(newValue, oldValue) {
        // Note: `newValue` will be equal to `oldValue` here
        // on nested mutations as long as the object itself
        // hasn't been replaced.
        // 注意：在嵌套的变更中，
        // 只要没有替换对象本身，
        // 那么这里的 `newValue` 和 `oldValue` 相同
      },
      deep: true
    }
  }
}
```

</div>

<div class="composition-api">

When you call `watch()` directly on a reactive object, it will implicitly create a deep watcher - the callback will be triggered on all nested mutations:

直接给 `watch()` 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的变更时都会被触发：

```js
const obj = reactive({ count: 0 })

watch(obj, (newValue, oldValue) => {
  // fires on nested property mutations
  // Note: `newValue` will be equal to `oldValue` here
  // because they both point to the same object!
  // 在嵌套的属性变更时触发
  // 注意：`newValue` 此处和 `oldValue` 是相等的
  // 因为它们是同一个对象！
})

obj.count++
```

This should be differentiated with a getter that returns a reactive object - in the latter case, the callback will only fire if the getter returns a different object:

相比之下，一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调：

```js
watch(
  () => state.someObject,
  () => {
    // fires only when state.someObject is replaced
    // 仅当 state.someObject 被替换时触发
  }
)
```

你也可以给上面这个例子显式地加上 `deep` 选项，强制转成深层侦听器：

```js
watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // Note: `newValue` will be equal to `oldValue` here
    // *unless* state.someObject has been replaced
    // 注意：`newValue` 此处和 `oldValue` 是相等的
    // *除非* state.someObject 被整个替换了
  },
  { deep: true }
)
```

</div>

In Vue 3.5+, the `deep` option can also be a number indicating the max traversal depth - i.e. how many levels should Vue traverse an object's nested properties.

在 Vue 3.5+ 中，`deep` 选项还可以是一个数字，表示最大遍历深度——即 Vue 应该遍历对象嵌套属性的级数。

:::warning Use with Caution | 谨慎使用
Deep watch requires traversing all nested properties in the watched object, and can be expensive when used on large data structures. Use it only when necessary and beware of the performance implications.

深度侦听需要遍历被侦听对象中的所有嵌套的属性，当用于大型数据结构时，开销很大。因此请只在必要时才使用它，并且要留意性能。
:::

## 即时回调的侦听器 | Eager Watchers {#eager-watchers}

`watch` is lazy by default: the callback won't be called until the watched source has changed. But in some cases we may want the same callback logic to be run eagerly - for example, we may want to fetch some initial data, and then re-fetch the data whenever relevant state changes.

`watch` 默认是懒执行的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望在创建侦听器时，立即执行一遍回调。举例来说，我们想请求一些初始数据，然后在相关状态更改时重新请求数据。

<div class="options-api">

We can force a watcher's callback to be executed immediately by declaring it using an object with a `handler` function and the `immediate: true` option:

我们可以用一个对象来声明侦听器，这个对象有 `handler` 方法和 `immediate: true` 选项，这样便能强制回调函数立即执行：

```js
export default {
  // ...
  watch: {
    question: {
      handler(newQuestion) {
        // this will be run immediately on component creation.
        // 在组件实例创建时会立即调用
      },
      // force eager callback execution
      // 强制立即执行回调
      immediate: true
    }
  }
  // ...
}
```

The initial execution of the handler function will happen just before the `created` hook. Vue will have already processed the `data`, `computed`, and `methods` options, so those properties will be available on the first invocation.

回调函数的初次执行就发生在 `created` 钩子之前。Vue 此时已经处理了 `data`、`computed` 和 `methods` 选项，所以这些属性在第一次调用时就是可用的。

</div>

<div class="composition-api">

We can force a watcher's callback to be executed immediately by passing the `immediate: true` option:

我们可以通过传入 `immediate: true` 选项来强制侦听器的回调立即执行：

```js
watch(
  source,
  (newValue, oldValue) => {
    // executed immediately, then again when `source` changes
    // 立即执行，且当 `source` 改变时再次执行
  },
  { immediate: true }
)
```

</div>

## 一次性侦听器 | Once Watchers {#once-watchers}

- Only supported in 3.4+
- 仅支持 3.4 及以上版本

Watcher's callback will execute whenever the watched source changes. If you want the callback to trigger only once when the source changes, use the `once: true` option.

每当被侦听源发生变化时，侦听器的回调就会执行。如果希望回调只在源变化时触发一次，请使用 `once: true` 选项。

<div class="options-api">

```js
export default {
  watch: {
    source: {
      handler(newValue, oldValue) {
        // when `source` changes, triggers only once
        // 当 `source` 变化时，仅触发一次
      },
      once: true
    }
  }
}
```

</div>

<div class="composition-api">

```js
watch(
  source,
  (newValue, oldValue) => {
    // when `source` changes, triggers only once
    // 当 `source` 变化时，仅触发一次
  },
  { once: true }
)
```

</div>

<div class="composition-api">

## `watchEffect()` \*\* {#watcheffect}

It is common for the watcher callback to use exactly the same reactive state as the source. For example, consider the following code, which uses a watcher to load a remote resource whenever the `todoId` ref changes:

侦听器的回调使用与源完全相同的响应式状态是很常见的。例如下面的代码，在每当 `todoId` 的引用发生变化时使用侦听器来加载一个远程资源：

```js
const todoId = ref(1)
const data = ref(null)

watch(
  todoId,
  async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    data.value = await response.json()
  },
  { immediate: true }
)
```

In particular, notice how the watcher uses `todoId` twice, once as the source and then again inside the callback.

特别是注意侦听器是如何两次使用 `todoId` 的，一次是作为源，另一次是在回调中。

This can be simplified with [`watchEffect()`](/api/reactivity-core#watcheffect). `watchEffect()` allows us to track the callback's reactive dependencies automatically. The watcher above can be rewritten as:

我们可以用 [`watchEffect` 函数](/api/reactivity-core#watcheffect) 来简化上面的代码。`watchEffect()` 允许我们自动跟踪回调的响应式依赖。上面的侦听器可以重写为：

```js
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

Here, the callback will run immediately, there's no need to specify `immediate: true`. During its execution, it will automatically track `todoId.value` as a dependency (similar to computed properties). Whenever `todoId.value` changes, the callback will be run again. With `watchEffect()`, we no longer need to pass `todoId` explicitly as the source value.

这个例子中，回调会立即执行，不需要指定 `immediate: true`。在执行期间，它会自动追踪 `todoId.value` 作为依赖（和计算属性类似）。每当 `todoId.value` 变化时，回调会再次执行。有了 `watchEffect()`，我们不再需要明确传递 `todoId` 作为源值。

You can check out [this example](/examples/#fetching-data) of `watchEffect()` and reactive data-fetching in action.

你可以参考一下[这个例子](/examples/#fetching-data)的 `watchEffect` 和响应式的数据请求的操作。

For examples like these, with only one dependency, the benefit of `watchEffect()` is relatively small. But for watchers that have multiple dependencies, using `watchEffect()` removes the burden of having to maintain the list of dependencies manually. In addition, if you need to watch several properties in a nested data structure, `watchEffect()` may prove more efficient than a deep watcher, as it will only track the properties that are used in the callback, rather than recursively tracking all of them.

对于这种只有一个依赖项的例子来说，`watchEffect()` 的好处相对较小。但是对于有多个依赖项的侦听器来说，使用 `watchEffect()` 可以消除手动维护依赖列表的负担。此外，如果你需要侦听一个嵌套数据结构中的几个属性，`watchEffect()` 可能会比深度侦听器更有效，因为它将只跟踪回调中被使用到的属性，而不是递归地跟踪所有的属性。

:::tip
`watchEffect` only tracks dependencies during its **synchronous** execution. When using it with an async callback, only properties accessed before the first `await` tick will be tracked.

`watchEffect` 仅会在其**同步**执行期间，才追踪依赖。在使用异步回调时，只有在第一个 `await` 正常工作前访问到的属性才会被追踪。
:::

### `watch` vs. `watchEffect` {#watch-vs-watcheffect}

`watch` and `watchEffect` both allow us to reactively perform side effects. Their main difference is the way they track their reactive dependencies:

`watch` 和 `watchEffect` 都能响应式地执行有副作用的回调。它们之间的主要区别是追踪响应式依赖的方式：

- `watch` only tracks the explicitly watched source. It won't track anything accessed inside the callback. In addition, the callback only triggers when the source has actually changed. `watch` separates dependency tracking from the side effect, giving us more precise control over when the callback should fire.

- `watch` 只追踪明确侦听的数据源。它不会追踪任何在回调中访问到的东西。另外，仅在数据源确实改变时才会触发回调。`watch` 会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机。

- `watchEffect`, on the other hand, combines dependency tracking and side effect into one phase. It automatically tracks every reactive property accessed during its synchronous execution. This is more convenient and typically results in terser code, but makes its reactive dependencies less explicit.

- `watchEffect`，则会在副作用发生期间追踪依赖。它会在同步执行过程中，自动追踪所有能访问到的响应式属性。这更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确。

</div>

## 副作用清理 | Side Effect Cleanup {#side-effect-cleanup}

Sometimes we may perform side effects, e.g. asynchronous requests, in a watcher:

有时我们可能会在侦听器中执行副作用，例如异步请求：

<div class="composition-api">

```js
watch(id, (newId) => {
  fetch(`/api/${newId}`).then(() => {
    // callback logic
    // 回调逻辑
  })
})
```

</div>
<div class="options-api">

```js
export default {
  watch: {
    id(newId) {
      fetch(`/api/${newId}`).then(() => {
        // callback logic
        // 回调逻辑
      })
    }
  }
}
```

</div>

But what if `id` changes before the request completes? When the previous request completes, it will still fire the callback with an ID value that is already stale. Ideally, we want to be able to cancel the stale request when `id` changes to a new value.

但是如果在请求完成之前 `id` 发生了变化怎么办？当上一个请求完成时，它仍会使用已经过时的 ID 值触发回调。理想情况下，我们希望能够在 `id` 变为新值时取消过时的请求。

We can use the [`onWatcherCleanup()`](/api/reactivity-core#onwatchercleanup) <sup class="vt-badge" data-text="3.5+" /> API to register a cleanup function that will be called when the watcher is invalidated and is about to re-run:

我们可以使用 [`onWatcherCleanup()`](/api/reactivity-core#onwatchercleanup) <sup class="vt-badge" data-text="3.5+" /> API 来注册一个清理函数，当侦听器失效并准备重新运行时会被调用：

<div class="composition-api">

```js {10-13}
import { watch, onWatcherCleanup } from 'vue'

watch(id, (newId) => {
  const controller = new AbortController()

  fetch(`/api/${newId}`, { signal: controller.signal }).then(() => {
    // callback logic
    // 回调逻辑
  })

  onWatcherCleanup(() => {
    // abort stale request
    // 终止过期请求
    controller.abort()
  })
})
```

</div>
<div class="options-api">

```js {12-15}
import { onWatcherCleanup } from 'vue'

export default {
  watch: {
    id(newId) {
      const controller = new AbortController()

      fetch(`/api/${newId}`, { signal: controller.signal }).then(() => {
        // callback logic
        // 回调逻辑
      })

      onWatcherCleanup(() => {
        // abort stale request
        // 终止过期请求
        controller.abort()
      })
    }
  }
}
```

</div>

Note that `onWatcherCleanup` is only supported in Vue 3.5+ and must be called during the synchronous execution of a `watchEffect` effect function or `watch` callback function: you cannot call it after an `await` statement in an async function.

请注意，`onWatcherCleanup` 仅在 Vue 3.5+ 中支持，并且必须在 `watchEffect` 效果函数或 `watch` 回调函数的同步执行期间调用：你不能在异步函数的 `await` 语句之后调用它。

Alternatively, an `onCleanup` function is also passed to watcher callbacks as the 3rd argument<span class="composition-api">, and to the `watchEffect` effect function as the first argument</span>:

作为替代，`onCleanup` 函数还作为第三个参数传递给侦听器回调<span class="composition-api">，以及 `watchEffect` 作用函数的第一个参数</span>：

<div class="composition-api">

```js
watch(id, (newId, oldId, onCleanup) => {
  // ...
  onCleanup(() => {
    // cleanup logic
    // 清理逻辑
  })
})

watchEffect((onCleanup) => {
  // ...
  onCleanup(() => {
    // cleanup logic
    // 清理逻辑
  })
})
```

</div>
<div class="options-api">

```js
export default {
  watch: {
    id(newId, oldId, onCleanup) {
      // ...
      onCleanup(() => {
        // cleanup logic
        // 清理逻辑
      })
    }
  }
}
```

</div>

This works in versions before 3.5. In addition, `onCleanup` passed via function argument is bound to the watcher instance so it is not subject to the synchronously constraint of `onWatcherCleanup`.

这在 3.5 之前的版本有效。此外，通过函数参数传递的 `onCleanup` 与侦听器实例相绑定，因此不受 `onWatcherCleanup` 的同步限制。

## 回调的触发时机 | Callback Flush Timing {#callback-flush-timing}

When you mutate reactive state, it may trigger both Vue component updates and watcher callbacks created by you.

当你更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调。

Similar to component updates, user-created watcher callbacks are batched to avoid duplicate invocations. For example, we probably don't want a watcher to fire a thousand times if we synchronously push a thousand items into an array being watched.

类似于组件更新，用户创建的侦听器回调函数也会被批量处理以避免重复调用。例如，如果我们同步将一千个项目推入被侦听的数组中，我们可能不希望侦听器触发一千次。

By default, a watcher's callback is called **after** parent component updates (if any), and **before** the owner component's DOM updates. This means if you attempt to access the owner component's own DOM inside a watcher callback, the DOM will be in a pre-update state.

默认情况下，侦听器回调会在父组件更新 (如有) **之后**、所属组件的 DOM 更新**之前**被调用。这意味着如果你尝试在侦听器回调中访问所属组件的 DOM，那么 DOM 将处于更新前的状态。

### Post Watchers {#post-watchers}

If you want to access the owner component's DOM in a watcher callback **after** Vue has updated it, you need to specify the `flush: 'post'` option:

如果想在侦听器回调中能访问被 Vue 更新**之后**的所属组件的 DOM，你需要指明 `flush: 'post'` 选项：

<div class="options-api">

```js{6}
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'post'
    }
  }
}
```

</div>

<div class="composition-api">

```js{2,6}
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
```

Post-flush `watchEffect()` also has a convenience alias, `watchPostEffect()`:

后置刷新的 `watchEffect()` 有个更方便的别名 `watchPostEffect()`：

```js
import { watchPostEffect } from 'vue'

watchPostEffect(() => {
  /* executed after Vue updates */
  /* 在 Vue 更新后执行 */
})
```

</div>

### 同步侦听器 | Sync Watchers {#sync-watchers}

It's also possible to create a watcher that fires synchronously, before any Vue-managed updates:

你还可以创建一个同步触发的侦听器，它会在 Vue 进行任何更新之前触发：

<div class="options-api">

```js{6}
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'sync'
    }
  }
}
```

</div>

<div class="composition-api">

```js{2,6}
watch(source, callback, {
  flush: 'sync'
})

watchEffect(callback, {
  flush: 'sync'
})
```

Sync `watchEffect()` also has a convenience alias, `watchSyncEffect()`:

同步触发的 `watchEffect()` 有个更方便的别名 `watchSyncEffect()`：

```js
import { watchSyncEffect } from 'vue'

watchSyncEffect(() => {
  /* executed synchronously upon reactive data change */
  /* 在响应式数据变化时同步执行 */
})
```

</div>

:::warning  Use with Caution | 谨慎使用
Sync watchers do not have batching and triggers every time a reactive mutation is detected. It's ok to use them to watch simple boolean values, but avoid using them on data sources that might be synchronously mutated many times, e.g. arrays.

同步侦听器不会进行批处理，每当检测到响应式数据发生变化时就会触发。可以使用它来监视简单的布尔值，但应避免在可能多次同步修改的数据源 (如数组) 上使用。
:::

<div class="options-api">

## `this.$watch()` \* {#this-watch}

It's also possible to imperatively create watchers using the [`$watch()` instance method](/api/component-instance#watch):

我们也可以使用组件实例的 [`$watch()` 方法](/api/component-instance#watch)来命令式地创建一个侦听器：

```js
export default {
  created() {
    this.$watch('question', (newQuestion) => {
      // ...
    })
  }
}
```

This is useful when you need to conditionally set up a watcher, or only watch something in response to user interaction. It also allows you to stop the watcher early.

如果要在特定条件下设置一个侦听器，或者只侦听响应用户交互的内容，这方法很有用。它还允许你提前停止该侦听器。

</div>

## 停止侦听器 | Stopping a Watcher {#stopping-a-watcher}

<div class="options-api">

Watchers declared using the `watch` option or the `$watch()` instance method are automatically stopped when the owner component is unmounted, so in most cases you don't need to worry about stopping the watcher yourself.

用 `watch` 选项或者 `$watch()` 实例方法声明的侦听器，会在宿主组件卸载时自动停止。因此，在大多数场景下，你无需关心怎么停止它。

In the rare case where you need to stop a watcher before the owner component unmounts, the `$watch()` API returns a function for that:

在少数情况下，你的确需要在组件卸载之前就停止一个侦听器，这时可以调用 `$watch()` API 返回的函数：

```js
const unwatch = this.$watch('foo', callback)

// ...when the watcher is no longer needed:
// ...当该侦听器不再需要时
unwatch()
```

</div>

<div class="composition-api">

Watchers declared synchronously inside `setup()` or `<script setup>` are bound to the owner component instance, and will be automatically stopped when the owner component is unmounted. In most cases, you don't need to worry about stopping the watcher yourself.

在 `setup()` 或 `<script setup>` 中用同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止。因此，在大多数情况下，你无需关心怎么停止一个侦听器。

The key here is that the watcher must be created **synchronously**: if the watcher is created in an async callback, it won't be bound to the owner component and must be stopped manually to avoid memory leaks. Here's an example:

一个关键点是，侦听器必须用**同步**语句创建：如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏。如下方这个例子：

```vue
<script setup>
import { watchEffect } from 'vue'

// this one will be automatically stopped
// 它会自动停止
watchEffect(() => {})

// ...this one will not!
// ...这个则不会！
setTimeout(() => {
  watchEffect(() => {})
}, 100)
</script>
```

To manually stop a watcher, use the returned handle function. This works for both `watch` and `watchEffect`:

要手动停止一个侦听器，请调用 `watch` 或 `watchEffect` 返回的函数：

```js
const unwatch = watchEffect(() => {})

// ...later, when no longer needed
// ...当该侦听器不再需要时
unwatch()
```

Note that there should be very few cases where you need to create watchers asynchronously, and synchronous creation should be preferred whenever possible. If you need to wait for some async data, you can make your watch logic conditional instead:

注意，需要异步创建侦听器的情况很少，请尽可能选择同步创建。如果需要等待一些异步数据，你可以使用条件式的侦听逻辑：

```js
// data to be loaded asynchronously
// 需要异步请求得到的数据
const data = ref(null)

watchEffect(() => {
  if (data.value) {
    // do something when data is loaded
    // 数据加载后执行某些操作...
  }
})
```

</div>

<!-- zhlint disabled -->
