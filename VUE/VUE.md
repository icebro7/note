# VUE



### ![image-20240331114542198](C:\Users\98680\Desktop\学习笔记\VUE\img\image-20240331114542198.png)



# 一、项目开始

## 1、终端命令

```powershell
1、创建项目
npm create vue@latest

2、安装依赖
npm i
npm i vite-plugin-vue-setup-extend -D

3、开启项目
npm run dev
```

## 2、config文件配置

```typescript
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'
import VueSetupExtend from 'vite-plugin-vue-setup-extend'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    vueJsx(),
    VueSetupExtend()
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```



# 二、语法结构

## 1、vue2编写格式

vue2可以读取vue3的内容，因为`setup`优先高

```html
#vue2

<template>
    <!-- heml -->


    <div class="person">
        <!-- 调用写好的方法 -->
        <h2>姓名：{{ name }}</h2>
        <h2>年龄：{{ age }}</h2>
        <button @click="showTel">查看tel</button>

    </div>

</template>

<script>
// js或ts
export default {
    // 创建方法并且赋值
    name: 'person',
    data() {
        return {
            name: '张三',
            age: 18,
            tel: 18559133133
        }
    },
    methods() {
        alert(this.tel)
    }
}
</script>

<style>
/* 样式 */
</style>
```



## 2、vue3编写格式

### 1、simple示例

```html

<!-- 组件api配置 -->
<script setup lang="ts" name="Persion">
let name = "zs"
function changeName() {
  name = "张三"
}
```



### 2、setup语法糖

```html
<script setup lang="ts">
  let a = 777
</script>
```



### 3、ref基本类型响应式数据

```vue
<script setup lang="ts">
import { ref } from 'vue'

let name = ref("zs")
let age = ref(18)

function changeName() {
  name.value = "张三"
}

function changeAge() {
  age.value += 1
  age.value = 28
}

</script>
```



### 4、reactive对象类型响应式数据

```vue
<script setup lang="ts">
import { reactive,toRefs } from 'vue'

let games = reactive({ name: "LOL", id: 1 })
let {name,id} = toRefs(games)	# 将原本响应式的每组元素拿出来变为响应式

function changeId() {
  games.id += 1
  Object.assign(games,{name:'ys',price:2})
}
</script>
```



## 3、ts编写格式

```typescript
// 引入createApp用于创建应用
import { createApp } from "vue";

// 引入app根主键
import App from "./App.vue";

// 调用传参
createApp(App).mount('#app')
```





