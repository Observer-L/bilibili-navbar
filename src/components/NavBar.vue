<template>
  <div class="navbar" :style="{top: navbarOffsetTop + 'px'}">
    <div class="navbar-list">
      <template v-for="(item, index) in floorData">
              <div 
                class="navbar-item sotrable" 
                :class="{'on' : current === index}"
                @click="scrollToFloor(index)"
                >{{item.name}}</div>
      </template>
      <div class="navbar-item customize">
        <div>↑↓</div>
        排序
      </div>
    </div>
    <div class="s-line"></div>
    <div class="back-top" @click="scrollToTop(time)">↑</div>
  </div>
</template>

<script>
import scrollMixin from './smooth-scroll.js';

  export default {
    mixins: [scrollMixin],
    props: {
      options: {
        type: Object
      }
    },
    data() {
      return {
        current: -1, // 当前到达/选中的楼层的位置，-1为最顶部
        floorData: [], // 储存楼层name、DOM element和OffsetTop等数据
        scrollTop: 0, // 当前页面位置距离顶部的距离
        navbarOffsetTop: 200, // navbar距离页面顶部的距离
        time: 500 // 滚动缓动动画时间
      }
    },
    methods: {
      // 初始化
      init() {
        this.initData();
        this.bindEvent();
      },
      initData() {
        // 初始化导航栏楼层数据floorData
        this.floorData = Array.from(this.options.items, (item) => {
          const element = document.getElementById(item.id);
          if (!element) {
            console.error(`找不到楼层 ${item.id} `);
            return;
          }
          const offsetTop = this.getOffsetTop(element);
          return {
            name: item.name,
            element: element,
            offsetTop: offsetTop,
          }
        })
      },
      bindEvent() {
        // 监听页面滚动
        document.addEventListener('scroll', this.scroll);
      },
      //获取每一层楼距离顶部的距离
      getOffsetTop(element) {
        let top, clientTop, clientLeft, scrollTop, scrollLeft,
          doc = document.documentElement,
          body = document.body
        if (typeof element.getBoundingClientRect !== "undefined") {
          top = element.getBoundingClientRect().top;
        } else {
          top = 0
        }
        clientTop = doc.clientTop || body.clientTop || 0
        scrollTop = window.pageYOffset || doc.scrollTop
        return (top + scrollTop - clientTop)
      },
      // 滚动事件处理函数
      scroll(e) {
        this.scrollTop = window.pageYOffset || (document.documentElement.scrollTop + document.body.scrollTop);
        // 判断是否已经滚动到该楼层，使用offset获得更好的交互体验
        for (let i = 0; i < this.floorData.length; i++) {
          if (this.scrollTop >= this.floorData[i].offsetTop - this.offset) {
            this.current = i;
          } else if (this.scrollTop < this.floorData[0].offsetTop - this.offset) {
            // 滚动到顶部则取消高亮
            this.current = -1;
          };
          // 设置整个navbarOffsetTop位置
          if (this.scrollTop >= this.offset) {
            this.navbarOffsetTop = 100;
          } else {
            this.navbarOffsetTop = 200;
          }
        }
      },
      scrollToFloor (index) {
        // 缓动滚动到指定楼层
        if (index === this.current) return;
        this.current = index;
        const target = this.floorData[index].element;
        this.scrollToElem(target, this.time, this.offset || 0);
      }
    },
    computed: {
      //  偏移值
      offset() {
        return this.options.offset || 100
      },
    },
    mounted() {
      this.init();
    }
  }
</script>

<style lang="less" scoped>
  .navbar {
    position: fixed;
    top: 200px;
    left: 50%;
    margin-left: 590px;
    transition: top .3s ease-in-out;
    .navbar-list {
      position: relative;
      background-color: #f6f9fa;
      border: 1px solid #e5e9ef;
      overflow: hidden;
      border-radius: 4px;
      .navbar-item {
        width: 48px;
        height: 32px;
        font-size: 12px;
        line-height: 32px;
        text-align: center;
        cursor: pointer;
        transition: background-color .3s, color .3s;
        user-select: none;
        &:hover,
        &.on {
          background-color: #00a1d6;
          color: #fff;
        }
        &.customize {
          height: 38px;
          line-height: 20px;
          padding: 8px 0;
          border-top: 1px solid #e5e9ef;
        }
      }
    }
    .back-top {
      position: relative;
      display: block;
      cursor: pointer;
      height: 48px;
      background-color: #f6f9fa;
      border: 1px solid #e5e9ef;
      overflow: hidden;
      border-radius: 4px;
      font-size: 24px;
      line-height: 48px;
      text-align: center;
      transition: background-color .3s, color .3s;
      user-select: none;
      &:hover {
        background-color: #00a1d6;
        color: #fff;
      }
    }
    .s-line {
      position: relative;
      border-left: 1px solid #ddd;
      border-right: 1px solid #ddd;
      height: 9px;
      width: 30px;
      margin: 0 auto;
    }
  }
</style>
