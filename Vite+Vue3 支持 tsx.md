### 配置

第一步 先安装 jsx 依赖 

```
yarn add @vitejs/plugin-vue-jsx -d or npm i @vitejs/plugin-vue-jsx -d
```

安装完成后如果报vite.createFilter is not a function查看vite版本和安装的依赖版本是否一致。

第二步 在vite.config.ts文件中引入并使用jsx依赖。

以下配置仅为了展示jsx配置展示，详细配置可查看 vite 详细配置 或 vite 官网

- jsx直接引入css文件少了sfc的scope会变成全局全局引入，vite使用了postcss-modules 可以开启模块化样式来避免样式冲突。样式命名需要加.modele，如果不想添加可参考 大佬博客。
- jsx的dom中变量需要以单层大括号包裹。
- 多层dom可以使用空标签包裹<></>。

```ts
import vue from '@vitejs/plugin-vue';
import vueJsx from '@vitejs/plugin-vue-jsx';
import type { UserConfig, ConfigEnv  } from "vite";

export default ({command, mode}: ConfigEnv): UserConfig => {
	return {
	    css: {
	      modules: {
	        scopeBehaviour: 'local', // 'global' | 'local'
	      }
	    },
		plugins: [
	      vue(),
	      vueJsx()
	    ]
	}
}
```

tsconfig.json include 配置添加.tsx。

```ts
{
	"include": [
    "src/**/*.tsx"
  ]
}
```

css配置完成后引入文件编辑器会爆红，可以在d.ts里面添加declare module '*.scss';

使用方式：

.tsx文件引入

```ts
import styles from 'test.module.scss'

export default defineComponent({
  name: 'test',
  setup () {
    return () => (
      <div class={styles['test-wrap']}></div>
    )
  }
})
```

### 基础使用

jsx一些事件的写法及指令需要改写

#### @click

`@click` 需要改成 `onClick`，当使用点击事件的时候需要写成 `onClick={(event) => toggleCate(event, data)}` 样式，当需要获取event事件时可以传值event不使用可为空

#### v-if

if else语句在使用时需要以大括号包裹

```tsx
export default defineComponent({
  name: 'test',
  setup () {
    const status = ref(true)
    return () => (
      <>
        { 
          status.value ? <div>1</div> : <div>2</div>
        }
      </>
    )
  }
})
```

#### v-for

```ts
export default defineComponent({
  name: 'test',
  setup () {
    const list = [1, 2, 3, 4]
    return () => (
      <>
        <div class={styles['test-wrap']}></div>

        {
          list.map(item => (
            <div>{item}</div>
          ))
        }
      </>
    )
  }
})
```

#### v-slot

在jsx里面的插槽需要使用v-slots插入。如果有传值可以在箭头函数中接收。slot传值写入当发现未渲染时记得查看这个值是不是可输出的，如果是对象会直接显示为空。

```ts
const footerSlot = {
  footer: () => (<div>
    <el-button>取消</el-button>
    <el-button type="primary">确定</el-button>
  </div>)
}
return () => (
	<el-dialog v-model={visible.value} title="测试" v-slots={footerSlot} before-close={closeDialog}></el-dialog>
)
```
