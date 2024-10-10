先上图

![image](https://github.com/user-attachments/assets/8fd6ad4a-7e9f-412e-becb-ccb16b8bf427)

官方源码的dialog.vue也看不出个什么大概来：

![image](https://github.com/user-attachments/assets/56f09f61-b901-481c-a88f-4c8bc0018b7a)

直接传入string，那传什么呢？class？id？，都不行，都会报错的。

![image](https://github.com/user-attachments/assets/c9cb907b-db12-4bfa-994c-5c541aac20b8)

那么想了一想，应该是直接将一个document的元素传进去，让它能识别到我要找到的元素；

那就开干：

```html
<template>
  <div id='render'>
    <el-button @click='visible = true'> open </el-button>
    <el-dialog :visible='visible' :append-to='render'>
      <div>我是示例代码</div>
    </el-dialog>
  </div>
</template>

<script lang='ts' setup>
import { ref, onMounted } from 'vue';

const visible = ref<boolean>(false);
const render = ref<HTMLDivElement | null>(null);

onMounted(() => {
  nextTick(() => {
    render.value = document.getElementById('render');
  });
});

</script>
```

原来这么简单，官方文档误人啊
