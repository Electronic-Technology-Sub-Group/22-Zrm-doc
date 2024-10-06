![image](https://github.com/user-attachments/assets/80821e2c-1d1f-47ea-b7b3-6e69ca55b9bc)

## 简介

Pinia 是 Vue3 的新一代状态管理库，提供了更简单的 API 和更好的 TypeScript 支持。它作为 Vuex 的替代方案，成为了管理 Vue 应用状态的首选。本文将详细介绍如何在 Vue3 项目中使用 Pinia 进行状态管理，并结合 pinia-plugin-persistedstate 插件实现状态持久化存储。

![image](https://github.com/user-attachments/assets/88c7b395-d614-495b-95b9-7d5615de5ded)


## Pinia 是什么

Pinia 是 Vue3 的新一代状态管理库。与 Vuex 相比，Pinia 提供了更简单的 API、更好的性能，并且完全支持 Vue3 的组合式 API 和 TypeScript。Pinia 的设计理念是尽量简化状态管理，减少不必要的复杂性，使开发者能够专注于业务逻辑的实现。

## 怎么安装Pinia

在 Vue3 项目中使用 Pinia，首先需要进行安装。

使用 npm 安装

```bash
npm install pinia
```

使用 yarn 安装

```bash
yarn add pinia
```

## 如何封装Pinia

安装完 Pinia 后，需要在项目中进行封装，以便全局使用。在 main.js 中引入并配置 Pinia。

```js
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';
const app = createApp(App);
const pinia = createPinia();
app.use(pinia);
app.mount('#app');
```

定义Store

在 Pinia 中，store 是用于管理状态的地方。下面是一个简单的示例，展示如何定义和使用一个 store。

```js
import { defineStore } from 'pinia';
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```

## Store三个核心概念

### 第一个核心概念State

> State 是存储数据的地方，它是 store 中的核心部分。

1. 使用state中的数据

在组件中使用 state 数据，可以通过 useStore 方法访问

```html
<!-- Counter.vue -->
<template>
  <div>
    <p>Count: {{ counter.count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>
<script>
import { useCounterStore } from '../stores/counter';
export default {
  setup() {
    const counter = useCounterStore();
    return { counter };
  }
};
</script>
```

2. 直接修改state中的数据

Pinia 允许直接修改 state 中的数据。

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```

3. $patch方法修改

可以使用 $patch 方法批量修改 state 中的数据。

```ts
const counter = useCounterStore();
counter.$patch({
  count: counter.count + 1
});
```

4. $state()方法替换state

使用 $state 方法可以直接替换整个 state。

```js
const counter = useCounterStore();
counter.$state = {
  count: 10
};
```

5. $reset() 方法重置state

使用 $reset 方法可以重置 state 到初始值。

```js
const counter = useCounterStore();
counter.$reset();
```

## 第二个核心概念Getters

Getters 类似于 Vue 的计算属性，用于计算派生状态。

定义getters

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  getters: {
    doubleCount(state) {
      return state.count * 2;
    }
  }
});
```

页面中使用getters

```html
<template>
  <div>
    <p>Double Count: {{ doubleCount }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>
<script>
import { useCounterStore } from '../stores/counter';
export default {
  setup() {
    const counter = useCounterStore();
    return {
      doubleCount: counter.doubleCount,
      increment: counter.increment
    };
  }
};
</script>
```

## 第三个核心概念Action

Actions 用于处理异步操作或封装复杂的业务逻辑。

定义action

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    },
    async incrementAsync() {
      await new Promise(resolve => setTimeout(resolve, 1000));
      this.increment();
    }
  }
});
```

使用action

```html
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
    <button @click="incrementAsync">Increment Async</button>
  </div>
</template>
<script>
import { useCounterStore } from '../stores/counter';
export default {
  setup() {
    const counter = useCounterStore();
    return {
      count: counter.count,
      increment: counter.increment,
      incrementAsync: counter.incrementAsync
    };
  }
};
</script>
```

## Pinia持久化存储

> 为了在页面刷新后保留状态，我们可以使用 pinia-plugin-persistedstate 插件来实现状态持久化存储

### 安装持久化插件pinia-plugin-persistedstate

1. 安装依赖

```bash
npm install pinia-plugin-persistedstate
```

2. 将插件添加到 pinia 实例上

在 main.js 中添加插件配置。

```js
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate';
import App from './App.vue';
const app = createApp(App);
const pinia = createPinia();
pinia.use(piniaPluginPersistedstate);
app.use(pinia);
app.mount('#app');
```

### 用法

### 基本使用

在 store 中启用状态持久化。

```js
import { defineStore } from 'pinia';
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  },
  persist: true
});
```

### 高级使用

可以自定义持久化的配置，如存储的 key 和使用的存储方式。

```js
import { defineStore } from 'pinia';
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  },
  persist: {
    key: 'my-counter',
    storage: localStorage,
    paths: ['count']
  }
});
```

### 高级配置示例

更复杂的配置示例：

```js
import { defineStore } from 'pinia';

export const useUserStore = defineStore('user', {
  state: () => ({
    name: '',
    age: 0,
    email: '',
    preferences: {
      theme: 'light',
      notifications: true
    }
  }),
  actions: {
    setUser(name, age, email) {
      this.name = name;
      this.age = age;
      this.email = email;
    },
    toggleNotifications() {
      this.preferences.notifications = !this.preferences.notifications;
    },
    setTheme(theme) {
      this.preferences.theme = theme;
    }
  },
  persist: {
    key: 'my-user-store',
    storage: localStorage, // 使用 localStorage 进行持久化
    paths: ['name', 'age', 'email', 'preferences.theme'] // 选择持久化的状态字段
  }
});
```
