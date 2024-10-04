在开发项目的时候经常会用到svg矢量图,而且我们使用SVG以后，页面上加载的不再是图片资源,

这对页面性能来说是个很大的提升，而且我们SVG文件比img要小的很多，放在项目中几乎不占用资源。

**安装SVG依赖插件**

```bash
pnpm install vite-plugin-svg-icons -D
```

**在`vite.config.ts`中配置插件**

```ts
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'
import path from 'path'
export default () => {
  return {
    plugins: [
      createSvgIconsPlugin({
        // Specify the icon folder to be cached
        iconDirs: [path.resolve(process.cwd(), 'src/assets/icons')],
        // Specify symbolId format
        symbolId: 'icon-[dir]-[name]',
      }),
    ],
  }
}
```

**入口文件导入**

```js
import 'virtual:svg-icons-register'
```

**svg封装为全局组件**

因为项目很多模块需要使用图标,因此把它封装为全局组件！！！

**在src/components目录下创建一个SvgIcon组件:代表如下**

```vue
<template>
  <div>
    <svg :style="{ width: width, height: height }">
      <use :xlink:href="prefix + name" :fill="color"></use>
    </svg>
  </div>
</template>

<script setup lang="ts">
defineProps({
  //xlink:href属性值的前缀
  prefix: {
    type: String,
    default: '#icon-'
  },
  //svg矢量图的名字
  name: String,
  //svg图标的颜色
  color: {
    type: String,
    default: ""
  },
  //svg宽度
  width: {
    type: String,
    default: '16px'
  },
  //svg高度
  height: {
    type: String,
    default: '16px'
  }

})
</script>
<style scoped></style>
```

在src文件夹目录下创建一个index.ts文件：用于注册components文件夹内部全部全局组件！！！

```ts
import SvgIcon from './SvgIcon/index.vue';
import type { App, Component } from 'vue';
const components: { [name: string]: Component } = { SvgIcon };
export default {
    install(app: App) {
        Object.keys(components).forEach((key: string) => {
            app.component(key, components[key]);
        })
    }
}
```

在入口文件引入src/index.ts文件,通过app.use方法安装自定义插件

```ts
import gloablComponent from './components/index';
app.use(gloablComponent);
```

**批量全局注册组件(代码优化)**

index.ts vue插件

```ts
import { App, Component } from 'vue'
import SvgIcon from '@/components/SvgIcon/index.vue'

// 定义类型
type ComponentType = {
    [key: string]: Component
}

// 所有组件储存在对象中
const allGlobalComponents: ComponentType = { SvgIcon }

export default {
  // vue插件的install方法
  install(app: App) {
    // 将对象的key转换成数组并进行便利
    Object.keys(allGlobalComponents).forEach((key: string) => {
      // 使用app的全局注册方法，key为组件名，进行全局注册组件
      app.component(key, allGlobalComponents[key])
    })
  },
}
```
