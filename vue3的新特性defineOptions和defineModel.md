## vue3.3 新特性 defineOptions

> 在`Vue3.3`之前，组件的默认组件名为`.vue`单文件组件文件的名字，假如我们想修改组件名，则需要结合`Options API`进行修改，`defineOptions` 的出现解决了这个问题。

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

## vue3.4 新特性defineModel

> 这个宏可以用来声明一个双向绑定 prop，通过父组件的 v-model 来使用。组件 v-model指南中也讨论了示例用法。在底层，这个宏声明了一个 model prop 和一个相应的值更新事件。如果第一个参数是一个字符串字面量，它将被用作 prop 名称；否则，prop 名称将默认为 "modelValue"。在这两种情况下，你都可以再传递一个额外的对象，它可以包含 prop 的选项和 model ref 的值转换选项
