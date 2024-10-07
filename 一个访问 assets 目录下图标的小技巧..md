![image](https://github.com/user-attachments/assets/9b009147-cd75-441e-a8b9-ff1a7ae6bf51)

如图没有显示出来, 是因为路径写错了

可以通过当前的 locallhost:端口 来访问我们这个 `assets` 目录下的静态资源

因此编码配置的时候, 就很方便了, 直接

```js
import { ILayoutMenuItem } from './interface'
export const layoutMenuList: ILayoutMenuItem[] = [
  {
    name: 'chat',
    path: '/chat',
    icon: '/src/assets/chat.svg',
  },
  {
    name: 'user',
    path: '/user',
    icon: '/src/assets/user.svg',
  },
  {
    name: 'dynamic',
    path: '/dynamic',
    icon: '/src/assets/dynamic.svg',
  },
]
```

页面渲染的时候, 就可以成功看到了

![image](https://github.com/user-attachments/assets/1f3dded7-a0f8-4865-b870-5517ca52f1cc)

其访问该资源的地址就是我的项目启动的服务+静态资源的位置
