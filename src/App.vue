<template>
  <div id="app">
    <div class="header"></div>
    <Floor :floors="floors"></Floor>
    <NavBar 
      :options="options" 
      :isSort="isSort" 
      @toggleMask="toggleMask"></NavBar>
    <div class="footer"></div>
    <transition name="fade">
      <div class="mask" v-if="isSort" @click="toggleMask(false)"></div>
    </transition>
  </div>
</template>

<script>
  import Floor from './components/Floor';
  import NavBar from './components/NavBar';
  
  export default {
    name: 'App',
    components: {
      Floor,
      NavBar
    },
    data() {
      return {
        isSort: false,
        floors: [{
          name: '直播',
          id: 'bili_live',
          sortindex: 0
        }, {
          name: '动画',
          id: 'bili_douga',
          sortindex: 1
        }, {
          name: '番剧',
          id: 'bili_bangumi',
          sortindex: 2
        }, {
          name: '国创',
          id: 'bili_guochuang',
          sortindex: 3
        }, {
          name: '音乐',
          id: 'bili_music',
          sortindex: 4
        }, {
          name: '音频',
          id: 'bili_audio',
          sortindex: 5
        }, {
          name: '舞蹈',
          id: 'bili_dance',
          sortindex: 6
        }, {
          name: '游戏',
          id: 'bili_game',
          sortindex: 7
        }, {
          name: '科技',
          id: 'bili_technology',
          sortindex: 8
        }, {
          name: '生活',
          id: 'bili_life',
          sortindex: 9
        }, {
          name: '鬼畜',
          id: 'bili_kichiku',
          sortindex: 10
        }, {
          name: '时尚',
          id: 'bili_fashion',
          sortindex: 11
        }, {
          name: '广告',
          id: 'bili_ad',
          sortindex: 12
        }, {
          name: '娱乐',
          id: 'bili_ent',
          sortindex: 13
        }, {
          name: '电影',
          id: 'bili_movie',
          sortindex: 14
        }, {
          name: 'TV剧',
          id: 'bili_teleplay',
          sortindex: 15
        }, {
          name: '影视',
          id: 'bili_cinephile',
          sortindex: 16
        }, {
          name: '纪录片',
          id: 'bili_documentary',
          sortindex: 17
        }, ]
      }
    },
    computed: {
      options() {
        let options = {
          offset: 200,
          offsetTop: 0,
          items: this.floors
        };
        return options;
      }
    },
    methods: {
      toggleMask: function (isSort) {
        // 是否开启排序模式和遮罩层
        this.isSort = isSort;
      }
    },
    watch: {
      // 保存位置到localStorage
      floors: function (arr) {
        let navCache = Array.from(arr, floor => floor.sortindex);
        localStorage.nav = JSON.stringify(navCache);
      }
    },
    mounted() {
      // 从localStorage获取navbar排序记录
      let navCache = localStorage.nav;
      if (navCache) {
        navCache = JSON.parse(navCache);
        const arr = [];
        Array.from(navCache, number => {
          arr.push(this.floors[number]);
        });
        this.floors = arr;
      }
    },
  }
</script>

<style>
  body {
    margin: 0;
    color: #000;
    font: 12px Helvetica Neue, Helvetica, Arial, Microsoft Yahei, Hiragino Sans GB, Heiti SC, WenQuanYi Micro Hei, sans-serif;
  }
  .header,
  .footer {
    min-width: 1160px;
    height: 300px;
    background: deepskyblue;
  }
  .mask {
    position: fixed;
    display: block;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 10000;
    top: 0;
    left: 0;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>
