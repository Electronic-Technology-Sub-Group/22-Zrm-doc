## animate.css在vue中的使用，路由动画transition或者在组件中控制使用

### 一、安装和引入

npm安装

```bash
npm install animate.css
```

在main.ts中引入

```ts
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus';
import 'element-plus/lib/theme-chalk/index.css';
import router from './router/index';
import store from "./store/index"
import * as echarts from 'echarts';
import Button from "@/components/Button.vue"

import 'animate.css/animate.min.css' //引入
// import 'animate.css' //这是官方的引入，但是会用不了


const app = createApp(App)
app.config.globalProperties.$echarts = echarts
app.component('Button',Button);
app.use(ElementPlus)
app.use(router)
app.use(store)
app.mount('#app')
```

### 二、使用步骤

#### 1.用于router-view

可直接用于transition中动画的使用：

template：

```html
<router-view v-slot="{ Component }">
    <transition name="router_animate">
      <component :is="Component" />
    </transition>
</router-view>
```

css:

```css
.router_animate-enter-active {
  animation: rollIn 1s;
}
.router_animate-leave-active {
  animation: rollOut 0.6s;
}
```

tips:类名直接用官方的动画名称就好了，比如下图：

![image](https://github.com/user-attachments/assets/a7e53cbf-ee0a-4525-a778-6a1404f7f302)

#### 2.用于组件

其实也就是控制类名是否加在元素上，值得注意的是，元素前面要加上animate__animated的前缀，下图：控制是否显示animate__bounce这个动画，flipinIn为true的时候

![image](https://github.com/user-attachments/assets/fea8a46d-6f3f-4325-ae6b-263f4d6faa3e)
