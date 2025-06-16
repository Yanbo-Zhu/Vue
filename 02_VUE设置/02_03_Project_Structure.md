
# 1 Project structure 


package.json: enthält alle Abhängigkeiten und Metadaten des Projekts.
node_modules: Verzeichnis, in dem installierte Pakete gespeichert werden

![](image/Pasted%20image%2020250106145623.png)

## 1.1 index.html
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + Vue</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```


## 1.2 main.js
```js
// 从 vue 这个 模块中 引入 createAPP 方法
import { createApp } from 'vue'
import './style.css'

// 导入 `App.vue` 文件中的默认 Vue 组件。`App.vue` 通常是应用的根组件。
import App from './App.vue'

// 使用 `createApp` 函数创建一个 Vue 应用，并将其挂载到 DOM 中 ID 为 `app` 的元素上。也就是说，Vue 应用将会在页面上呈现在这个元素内
// id=app 在 index.html 中 
createApp(App).mount('#app')
```


## 1.3 App.vue  (这个是整个 project 的根文件)

App.vue  是vue 提供的一个组件 

```vue
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

```


## 1.4 HelloWorld.vue
```vue
<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

```


# 2 export default 的用法

Hello.vue
```vue
<!-- Hello.vue -->
<template>
  <div>
    <h1>你好，世界！</h1>
  </div>
</template>

<script>
export default {
  name: 'Hello'
}
</script>

```


然后，在 App.vue 中，你需要导入 Hello.vue 组件并将其注册到 App 组件中。完整代码如下：

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <Hello />  <!-- 在这里使用 Hello 组件 -->
  </div>
</template>

<script>
import Hello from './Hello.vue';  // 导入 Hello 组件

export default {
  name: 'App',  // 设置组件名称
  components: {
    Hello  // 注册 Hello 组件
  }
}
</script>

```

1. **导入 `Hello` 组件**：在 `App.vue` 文件中，通过 `import Hello from './Hello.vue';` 导入了 `Hello.vue` 组件。
2. **注册 `Hello` 组件**：在 `components` 对象中注册 `Hello`，这样 `App` 组件就可以在模板 `<template> </tample>`中使用 `<Hello />` 作为标签来渲染 `Hello` 组件。
3. **使用 `Hello` 组件**：在 `App.vue` 的模板部分使用 `<Hello />` 标签来嵌套 `Hello` 组件。

- **`export default`**：将这个配置对象作为默认导出，通常在 Vue.js 中使用这种方式导出组件。
- **`name: 'App'`**：为组件指定一个名称 `"App"`，这对于调试和在 Vue 开发工具中识别组件非常有用。
- **`components: { Hello }`**：在 `App` 组件中注册并使用名为 `Hello` 的子组件。你需要先导入 `Hello` 组件。

当你运行这个 Vue 应用时，`App` 组件会渲染 `Hello` 组件的内容，即显示 `"你好，世界！"`。

这个例子演示了如何在 Vue 中使用组件嵌套，让你能够构建更为复杂的应用结构。