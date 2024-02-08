cesium入门

# 第一章：前言-基础知识

一个跨平台、跨浏览器的展示三维地球和地图的JavaScript库

## 课程原则

- 入门为主，更加深入的学习可以登入cesium的官方网
- 每次讲解不超过十分钟
- 需要初步了解html，css，js，vue3。其他的跟着敲代码就行了
- 最终达成效果

## 课程目录

- 第一章：前言
    
    > 基础知识
    
- 第二章：软件安装
    
- 第三章：开始编程
    
    > 1、把地球显示出来  
    > 2、获取相机位置  
    > 3、设置相机位置  
    > 4、加载在线地图
    > 
    > - 天地图说明http://lbs.tianditu.gov.cn/server/MapService.html
    > 
    > 5、添加点  
    > 6、添加边界  
    > 7、可视化
    
- 第四章：总结
    

## 基础知识

### 了解一下坐标系

- 经纬度表示，84坐标系，火星坐标系，bd09坐标系，2000 坐标系  
    <img src="https://static.oschina.net/uploads/space/2014/0714/124613_sr4k_1585572.png" alt="经纬度表示" width="402" height="305" class="jop-noMdConv">
- 笛卡尔空间，  
    <img src="https://img2018.cnblogs.com/blog/48590/201908/48590-20190802171817441-773828282.jpg" alt="笛卡尔" width="442" height="442" class="jop-noMdConv">
- 入门课程，只需要知道，不需要了解

# 第二章：软件安装

- vs code：宇宙第一IDE
- node.js: 用于安装、管理和共享JavaScript模块
- cnpm : 国内源

```
npm install -g cnpm --registry=https://registry.npmmirror.com
```

- vue3 ： JavaScript框架

```
cnpm create vite@latest name -- --template vue
cd test2
cnpm install
cnpm run dev

# 展示内容，浏览器可直接访问
  VITE v5.0.10  ready in 1442 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

- element-plus：组件库

```
npm install element-plus --save
```

修改main.js、修改后

```javascrpt
import { createApp } from 'vue'
// 新增
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)
// 新增
app.use(ElementPlus)
app.mount('#app')
```

- cesium、vite-plugin-cesium：组件

```
cnpm install cesium vite-plugin-cesium
```

配置vite.config.js

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import cesium from 'vite-plugin-cesium'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),cesium()],
})
```

# 第三章：开始编程

实际操作


# 第四章：总结

- 特别浅显，没有涉及到动画制作之类。
- 随后放到github上，如果有需要可以自行下载
- 交流，可视化前沿。
