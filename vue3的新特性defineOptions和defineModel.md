## vue3.3 新特性 defineOptions

> 在 `Vue3.3` 之前，组件的默认组件名为 `.vue` 单文件组件文件的名字，假如我们想修改组件名，则需要结合 `Options API` 进行修改，`defineOptions` 的出现解决了这个问题。

这个宏可以用来直接在 `<script setup>` 中声明组件选项，而不必使用单独的 `<script>` 块：

```ts
<script setup lang="ts">
defineOptions({
  name: '组件名称'
  inheritAttrs: false, // 组件标签上的属性是否透传
  customOptions: {
    /* 其他配置 */
  }
})
</script>
```

## vue3.4 新特性 defineModel

> 这个宏可以用来声明一个双向绑定 `prop`，通过父组件的 `v-model` 来使用。组件 `v-model` 指南中也讨论了示例用法。在底层，这个宏声明了一个 `model prop` 和一个相应的值更新事件。如果第一个参数是一个字符串字面量，它将被用作 `prop` 名称；否则，`prop` 名称将默认为 `"modelValue"`。在这两种情况下，你都可以再传递一个额外的对象，它可以包含 `prop` 的选项和 `model ref` 的值转换选项。

### 简介

defineModel() 返回的值是一个 ref。它可以像其他 ref 一样被访问以及修改，不过它能起到在父组件和当前变量之间的双向绑定的作用：

- 它的 value 和父组件的 v-model 的值同步；

- 当它被子组件变更了，会触发父组件绑定的值一起更新。

### 底层机制

defineModel 是一个编译宏。 编译器将其展开为以下内容：

- 一个名为 modelValue 的 prop，本地 ref 的值与其同步；
- 一个名为 update:modelValue 的事件，当本地 ref 的值发生变更时触发。

### 默认v-model

父组件Parent.vue

```html
<template>
  <Child v-model="value"></Child>
</template>
<script setup lang="ts">
import { ref } from "vue";
const value = ref("");
</script>
```

子组件Child.vue

1、vue3.4前用法

```html
<template>
  <input
    :value="modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
<script setup lang="ts">
const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);
</script>
```

2、vue3.4用法 

```html
<template>
  <input type="text" v-model="model" />
</template>
<script setup lang="ts">
// model变量名称可随意取
const model = defineModel();
</script>
```

### 带参数的v-model

父组件Parent.vue

```html
<template>
  <Child v-model:date="dateValue"></Child>
</template>
<script setup lang="ts">
import { ref } from "vue";
const dateValue = ref("");
</script>
```

子组件Child.vue

1、vue3.4前用法

```html
<template>
  <input
    type="text"
    :value="date"
    @input="emit('update:date', $event.target.value)"
  />
</template>
<script setup lang="ts">
const props = defineProps(["date"]);
const emit = defineEmits(["update:date"]);
</script>
```

2、vue3.4用法 

```html
<template>
  <input type="text" v-model="date" />
</template>
<script setup lang="ts">
const name = defineModel("date");
</script>
```

### 同时绑定多个v-model

父组件Parent.vue

```html
<template>
  <Child v-model:date="dateValue" v-model:address="dateTimeValue"></Child>
</template>
<script setup lang="ts">
import { ref } from "vue";
const dateValue = ref("");
const dateTimeValue = ref("");
</script>
```

子组件Child.vue

1、vue3.4前用法

```html
<template>
  <input
    type="text"
    :value="date"
    @input="$emit('update:date', $event.target.value)"
  />
  <input
    type="text"
    :value="dateTime"
    @input="$emit('update:dateTime', $event.target.value)"
  />
</template>
<script setup lang="ts">
defineProps(["date", "dateTime"]);
defineEmits(["update:date", "update:dateTime"]);
</script>
```

2、vue3.4用法 

```html
<template>
  <input type="text" v-model="dateValue" />
  <input type="text" v-model="dateTime" />
</template>
<script setup lang="ts">
const dateValue = defineModel("date");
const dateTimeValue = defineModel("dateTime");
</script>
```
