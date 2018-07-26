<template>
  <div class="navbar" :style="{top: navbarOffsetTop + 'px'}" :class="{ 'edit-state' : isSort }">
    <div class="nav-bg" :class="{'open' : isSort}">
      <div class="tips-img"></div>
    </div>
    <div class="navbar-list" ref="list">
      <template v-for="(item, index) in floorData">
        <div class="navbar-item sortable" v-if="isDrag && index === replaceItem && replaceItem <= dragId"></div>
        <div 
          class="navbar-item sotrable" 
          :class="[{'on' : current === index && !isSort}, {'drag': isDrag && current === index}]"
          @click="scrollToFloor(index)"
          @mousedown="startDrag($event, index)"
          :style="dragStyles"
          >{{item.name}}</div>
          <div class="navbar-item sortable" v-if="isDrag && index === replaceItem && replaceItem > dragId"></div>
      </template>
      <div class="navbar-item customize" @click="activeSort">
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
      },
      isSort: {
        // 是否开启排序模式和遮罩层
        type: Boolean
      }
    },
    data() {
      return {
        current: -1, // 当前到达/选中的楼层的位置，-1为最顶部
        floorData: [], // 储存楼层name、DOM element和OffsetTop等数据
        scrollTop: 0, // 当前页面位置距离顶部的距离
        navbarOffsetTop: 200, // navbar距离页面顶部的距离
        time: 500, // 滚动缓动动画时间
        isDrag: false, // 是否在拖拽状态
        dragId: 0, // 拖拽楼层号
        offsetY: 0, // 鼠标在要拖拽的元素上的Y坐标上的偏移
        y: 0, // 被拖拽的元素在其相对的元素上的Y坐标上的偏移
        height: 32, // 导航栏单层的高度
      }
    },
    methods: {
      // 初始化
      init() {
        this.initData();
        document.addEventListener('scroll', this.scroll);
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
        document.addEventListener('mousemove', this.dragMove);
        document.addEventListener('mouseup', this.dragEnd);
        document.addEventListener('mouseleave', this.dragEnd);
      },
      unbindEvent() {
        document.removeEventListener('mousemove', this.dragMove);
        document.removeEventListener('mouseup', this.dragEnd);
        document.removeEventListener('mouseleave', this.dragEnd);
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
          this.scrollTop >= this.offset ? this.navbarOffsetTop = 100 : this.navbarOffsetTop = 200;
        }
      },
      scrollToFloor(index) {
        // 缓动滚动到指定楼层
        if (index === this.current) return;
        this.current = index;
        const target = this.floorData[index].element;
        this.scrollToElem(target, this.time, this.offset || 0);
      },
      activeSort() {
        // 切换排序模式
        // 向父组件传值，切换遮罩层和排序模式
        this.isSort === true ? this.unbindEvent() : this.bindEvent();
        this.$emit('toggleMask', !this.isSort);
      },
      startDrag(e, index) {
        // 排序模式下拖动导航栏楼层时，获取当前选中楼层ID和鼠标Y轴偏移
        if (!this.isSort) return;
        this.isDrag = true;
        this.current = index;
        this.dragId = index;
        this.offsetY = e.offsetY;
        this.getPos(e);
      },
      dragMove(e) {
        e.preventDefault();
        return this.isDrag === true ? this.getPos(e) : false;
      },
      dragEnd() {
        if (this.isDrag) {
          this.isDrag = false;
          if (this.replaceItem !== this.dragId) {
            this.options.items.splice(this.replaceItem, 0, this.options.items.splice(this.dragId, 1)[0]);
          } else {
            this.scrollToFloor(this.dragId);
          }
        }
      },
      getPos(e) {
        // 获取当前选中楼层相对于导航栏的y轴偏移
        this.y = e.clientY - this.navbarOffsetTop - this.offsetY;
      }
    },
    computed: {
      //  偏移值
      offset() {
        return this.options.offset || 100
      },
      // 拖拽的元素的position会变为absolute,dragStyles用来设置其位置,鼠标运动时会调用,从而实现跟随鼠标运动的功能
      dragStyles() {
        return {
          top: `${this.y}px`
        }
      },
      //当被拖拽的元素运动到其他元素的位置时,会使得replaceItem发送变化
      replaceItem() {
        let id = Math.floor(this.y / this.height);
        if (id > this.floorData.length - 1)
          id = this.floorData.length;
        if (id < 0)
          id = 0;
        return id;
      }
    },
    mounted() {
      this.init();
    },
    beforeDestroy() {
      document.removeEventListener('scroll', this.scroll);
      this.unbindEvent();
    },
    watch: {
      //监听options的变化
      options: {
        deep: true, // 深度监听
        handler(newVal, oldVal) {
          this.initData();
        }
      }
    },
  }
</script>

<style lang="less" scoped>
  .navbar {
    position: fixed;
    top: 200px;
    left: 50%;
    margin-left: 590px;
    transition: top .3s ease-in-out;
    &.edit-state {
      z-index: 10001;
    }
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
        &.drag {
          position: absolute;
          height: 32px;
          width: 100%;
          background: #00a1d6;
          color: #fff;
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
    .nav-bg {
      position: absolute;
      top: -15px;
      right: 0;
      width: 60px;
      height: 100%;
      padding-bottom: 20px;
      overflow: hidden;
      border-radius: 4px;
      opacity: 0;
      background: hsla(0, 0%, 100%, .8);
      transition: all .3s cubic-bezier(.68, -.55, .27, 1.55);
      &.open {
        right: -20px;
        width: 200px;
        opacity: 1;
      }
      .tips-img {
        position: absolute;
        width: 117px;
        height: 333px;
        background: url(../assets/tab2233.png);
        left: 12px;
        top: 14px;
      }
    }
  }
</style>
