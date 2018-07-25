# bilibili-navbar

> B站首页右侧导航条实现

## To-do List
 ✅ 滚动高亮  
 ✅ 返回顶部  
 ✅ 楼层切换  
 ✅ 楼层排序  
 🔲 弹出层特效  
 🔲 排序记录本地储存

## 待解决的问题
1. 楼层排序拖动时，加一个offset而不用整个拖过上下楼层
2. 拖动排序后，高亮的是最后点击排序的楼层，而不是当前页面所在位置的楼层
3. 拖拽事件只要在排序模式激活下绑定，退出后解绑，需要优化性能

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
