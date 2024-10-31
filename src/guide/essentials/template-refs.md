# 模板引用 Template Refs {#template-refs}

While Vue's declarative rendering model abstracts away most of the direct DOM operations for you, there may still be cases where we need direct access to the underlying DOM elements. To achieve this, we can use the special `ref` attribute:

虽然 Vue 的声明性渲染模型为你抽象了大部分对 DOM 的直接操作，但在某些情况下，我们仍然需要直接访问底层 DOM 元素。要实现这一点，我们可以使用特殊的 `ref` attribute：

```vue-html
<input ref="input">
```

`ref` is a special attribute, similar to the `key` attribute discussed in the `v-for` chapter. It allows us to obtain a direct reference to a specific DOM element or child component instance after it's mounted. This may be useful when you want to, for example, programmatically focus an input on component mount, or initialize a 3rd party library on an element.

`ref` 是一个特殊的 attribute，和 `v-for` 章节中提到的 `key` 类似。它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用。这可能很有用，比如说在组件挂载时将焦点设置到一个 input 元素上，或在一个元素上初始化一个第三方库。

## 访问模板引用 | Accessing the Refs {#accessing-the-refs}

<div class="composition-api">

To obtain the reference with Composition API, we can use the [`useTemplateRef()`](/api/composition-api-helpers#usetemplateref) <sup class="vt-badge" data-text="3.5+" /> helper:

要在组合式 API 中获取引用，我们可以使用辅助函数 [`useTemplateRef()`](/api/composition-api-helpers#usetemplateref) <sup class="vt-badge" data-text="3.5+" />：

```vue
<script setup>
import { useTemplateRef, onMounted } from 'vue'

// the first argument must match the ref value in the template
// 第一个参数必须与模板中的 ref 值匹配
const input = useTemplateRef('my-input')

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="my-input" />
</template>
```

When using TypeScript, Vue's IDE support and `vue-tsc` will automatically infer the type of `input.value` based on what element or component the matching `ref` attribute is used on.

在使用 TypeScript 时，Vue 的 IDE 支持和 `vue-tsc` 将根据匹配的 `ref` attribute 所用的元素或组件自动推断 `inputRef.value` 的类型。

<details>
<summary>3.5 前的用法 | Usage before 3.5</summary>

In versions before 3.5 where `useTemplateRef()` was not introduced, we need to declare a ref with a name that matches the template ref attribute's value:

在 3.5 之前的版本尚未引入 `useTemplateRef()`，我们需要声明一个与模板里 ref attribute 匹配的引用：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// declare a ref to hold the element reference
// 声明一个 ref 来存放该元素的引用
// the name must match template ref value
// 必须和模板里的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

If not using `<script setup>`, make sure to also return the ref from `setup()`:

如果不使用 `<script setup>`，需确保从 `setup()` 返回 ref：

```js{6}
export default {
  setup() {
    const input = ref(null)
    // ...
    return {
      input
    }
  }
}
```

</details>

</div>
<div class="options-api">

The resulting ref is exposed on `this.$refs`:

挂载结束后引用都会被暴露在 `this.$refs` 之上：

```vue
<script>
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
</script>

<template>
  <input ref="input" />
</template>
```

</div>

Note that you can only access the ref **after the component is mounted.** If you try to access <span class="options-api">`$refs.input`</span><span class="composition-api">`input`</span> in a template expression, it will be <span class="options-api">`undefined`</span><span class="composition-api">`null`</span> on the first render. This is because the element doesn't exist until after the first render!

注意，你只可以**在组件挂载后**才能访问模板引用。如果你想在模板中的表达式上访问 <span class="options-api">`$refs.input`</span><span class="composition-api">`input`</span>，在初次渲染时会是 <span class="options-api">`undefined`</span><span class="composition-api">`null`</span>。这是因为在初次渲染前这个元素还不存在呢！

<div class="composition-api">

If you are trying to watch the changes of a template ref, make sure to account for the case where the ref has `null` value:

如果你需要侦听一个模板引用 ref 的变化，确保考虑到其值为 `null` 的情况：

```js
watchEffect(() => {
  if (input.value) {
    input.value.focus()
  } else {
    // not mounted yet, or the element was unmounted (e.g. by v-if)
    // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
  }
})
```

See also: [Typing Template Refs](/guide/typescript/composition-api#typing-template-refs) <sup class="vt-badge ts" />

也可参考：[为模板引用标注类型](/guide/typescript/composition-api#typing-template-refs) <sup class="vt-badge ts" />

</div>

## `v-for` 中的模板引用 | Refs inside `v-for` {#refs-inside-v-for}

> Requires v3.5 or above
>
> 需要 v3.2.25 及以上版本

<div class="composition-api">

When `ref` is used inside `v-for`, the corresponding ref should contain an Array value, which will be populated with the elements after mount:

当在 `v-for` 中使用模板引用时，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素：

```vue
<script setup>
import { ref, useTemplateRef, onMounted } from 'vue'

const list = ref([
  /* ... */
])

const itemRefs = useTemplateRef('items')

onMounted(() => console.log(itemRefs.value))
</script>

<template>
  <ul>
    <li v-for="item in list" ref="items">
      {{ item }}
    </li>
  </ul>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNp9UsluwjAQ/ZWRLwQpDepyQoDUIg6t1EWUW91DFAZq6tiWF4oU5d87dtgqVRyyzLw3b+aN3bB7Y4ptQDZkI1dZYTw49MFMuBK10dZDAxZXOQSHC6yNLD3OY6zVsw7K4xJaWFldQ49UelxxVWnlPEhBr3GszT6uc7jJ4fazf4KFx5p0HFH+Kme9CLle4h6bZFkfxhNouAIoJVqfHQSKbSkDFnVpMhEpovC481NNVcr3SaWlZzTovJErCqgydaMIYBRk+tKfFLC9Wmk75iyqg1DJBWfRxT7pONvTAZom2YC23QsMpOg0B0l0NDh2YjnzjpyvxLrYOK1o3ckLZ5WujSBHr8YL2gxnw85lxEop9c9TynkbMD/kqy+svv/Jb9wu5jh7s+jQbpGzI+ZLu0byEuHZ+wvt6Ays9TJIYl8A5+i0DHHGjvYQ1JLGPuOlaR/TpRFqvXCzHR2BO5iKg0Zmm/ic0W2ZXrB+Gve2uEt1dJKs/QXbwePE)

<details>
<summary>3.5 前的用法 | Usage before 3.5</summary>

```vue
<script setup>
import { ref, onMounted } from 'vue'

const list = ref([
  /* ... */
])

const itemRefs = ref([])

onMounted(() => console.log(itemRefs.value))
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRefs">
      {{ item }}
    </li>
  </ul>
</template>
```

</details>

</div>
<div class="options-api">

When `ref` is used inside `v-for`, the resulting ref value will be an array containing the corresponding elements:

当在 `v-for` 中使用模板引用时，相应的引用中包含的值是一个数组：

```vue
<script>
export default {
  data() {
    return {
      list: [
        /* ... */
      ]
    }
  },
  mounted() {
    console.log(this.$refs.items)
  }
}
</script>

<template>
  <ul>
    <li v-for="item in list" ref="items">
      {{ item }}
    </li>
  </ul>
</template>
```

[在演练场中尝试一下 | Try it in the Playground](https://play.vuejs.org/#eNpFjk0KwjAQha/yCC4Uaou6kyp4DuOi2KkGYhKSiQildzdNa4WQmTc/37xeXJwr35HEUdTh7pXjszT0cdYzWuqaqBm9NEDbcLPeTDngiaM3PwVoFfiI667AvsDhNpWHMQzF+L9sNEztH3C3JlhNpbaPNT9VKFeeulAqplfY5D1p0qurxVQSqel0w5QUUEedY8q0wnvbWX+SYgRAmWxIiuSzm4tBinkc6HvkuSE7TIBKq4lZZWhdLZfE8AWp4l3T)

</div>

It should be noted that the ref array does **not** guarantee the same order as the source array.

应该注意的是，ref 数组**并不**保证与源数组相同的顺序。

## 函数模板引用 | Function Refs {#function-refs}

Instead of a string key, the `ref` attribute can also be bound to a function, which will be called on each component update and gives you full flexibility on where to store the element reference. The function receives the element reference as the first argument:

除了使用字符串值作名字，`ref` attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：

```vue-html
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 | assign el to a property or ref */ }">
```

Note we are using a dynamic `:ref` binding so we can pass it a function instead of a ref name string. When the element is unmounted, the argument will be `null`. You can, of course, use a method instead of an inline function.

注意我们这里需要使用动态的 `:ref` 绑定才能够传入一个函数。当绑定的元素被卸载时，函数也会被调用一次，此时的 `el` 参数会是 `null`。你当然也可以绑定一个组件方法而不是内联函数。

## 组件上的 ref | Ref on Component {#ref-on-component}

> This section assumes knowledge of [Components](/guide/essentials/component-basics). Feel free to skip it and come back later.
>
> 这一小节假设你已了解[组件](/guide/essentials/component-basics)的相关知识，或者你也可以先跳过这里，之后再回来看。

`ref` can also be used on a child component. In this case the reference will be that of a component instance:

模板引用也可以被用在一个子组件上。这种情况下引用中获得的值是组件实例：

<div class="composition-api">

```vue
<script setup>
import { useTemplateRef, onMounted } from 'vue'
import Child from './Child.vue'

const childRef = useTemplateRef('child')

onMounted(() => {
  // childRef.value will hold an instance of <Child />
  // childRef.value 将持有 <Child /> 的实例
})
</script>

<template>
  <Child ref="child" />
</template>
```

<details>
<summary>3.5 前的用法 | Usage before 3.5</summary>

```vue
<script setup>
import { ref, onMounted } from 'vue'
import Child from './Child.vue'

const child = ref(null)

onMounted(() => {
  // child.value will hold an instance of <Child />
  // child.value 是 <Child /> 组件的实例
})
</script>

<template>
  <Child ref="child" />
</template>
```

</details>

</div>
<div class="options-api">

```vue
<script>
import Child from './Child.vue'

export default {
  components: {
    Child
  },
  mounted() {
    // this.$refs.child will hold an instance of <Child />
    // this.$refs.child 是 <Child /> 组件的实例
  }
}
</script>

<template>
  <Child ref="child" />
</template>
```

</div>

<span class="composition-api">If the child component is using Options API or not using `<script setup>`, the</span><span class="options-api">The</span> referenced instance will be identical to the child component's `this`, which means the parent component will have full access to every property and method of the child component. This makes it easy to create tightly coupled implementation details between the parent and the child, so component refs should be only used when absolutely needed - in most cases, you should try to implement parent / child interactions using the standard props and emit interfaces first.

如果一个子组件使用的是选项式 API <span class="composition-api">或没有使用 `<script setup>`</span>，被引用的组件实例和该子组件的 `this` 完全一致，这意味着父组件对子组件的每一个属性和方法都有完全的访问权。这使得在父组件和子组件之间创建紧密耦合的实现细节变得很容易，当然也因此，应该只在绝对需要时才使用组件引用。大多数情况下，你应该首先使用标准的 props 和 emit 接口来实现父子组件交互。

<div class="composition-api">

An exception here is that components using `<script setup>` are **private by default**: a parent component referencing a child component using `<script setup>` won't be able to access anything unless the child component chooses to expose a public interface using the `defineExpose` macro:

有一个例外的情况，使用了 `<script setup>` 的组件是**默认私有**的：一个父组件无法访问到一个使用了 `<script setup>` 的子组件中的任何东西，除非子组件在其中通过 `defineExpose` 宏显式暴露：

```vue
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

// Compiler macros, such as defineExpose, don't need to be imported
// 像 defineExpose 这样的编译器宏不需要导入
defineExpose({
  a,
  b
})
</script>
```

When a parent gets an instance of this component via template refs, the retrieved instance will be of the shape `{ a: number, b: number }` (refs are automatically unwrapped just like on normal instances).

当父组件通过模板引用获取到了该组件的实例时，得到的实例类型为 `{ a: number, b: number }` (ref 都会自动解包，和一般的实例一样)。

See also: [Typing Component Template Refs](/guide/typescript/composition-api#typing-component-template-refs) <sup class="vt-badge ts" />

TypeScript 用户请参考：[为组件的模板引用标注类型](/guide/typescript/composition-api#typing-component-template-refs) <sup class="vt-badge ts" />

</div>
<div class="options-api">

The `expose` option can be used to limit the access to a child instance:

`expose` 选项可以用于限制对子组件实例的访问：

```js
export default {
  expose: ['publicData', 'publicMethod'],
  data() {
    return {
      publicData: 'foo',
      privateData: 'bar'
    }
  },
  methods: {
    publicMethod() {
      /* ... */
    },
    privateMethod() {
      /* ... */
    }
  }
}
```

In the above example, a parent referencing this component via template ref will only be able to access `publicData` and `publicMethod`.

在上面这个例子中，父组件通过模板引用访问到子组件实例后，仅能访问 `publicData` 和 `publicMethod`。

</div>
