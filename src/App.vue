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
          id: 'bili_live'
        }, {
          name: '动画',
          id: 'bili_douga'
        }, {
          name: '番剧',
          id: 'bili_bangumi'
        }, {
          name: '国创',
          id: 'bili_guochuang'
        }, {
          name: '音乐',
          id: 'bili_music'
        }, {
          name: '音频',
          id: 'bili_audio'
        }, {
          name: '舞蹈',
          id: 'bili_dance'
        }, {
          name: '游戏',
          id: 'bili_game'
        }, {
          name: '科技',
          id: 'bili_technology'
        }, {
          name: '生活',
          id: 'bili_life'
        }, {
          name: '鬼畜',
          id: 'bili_kichiku'
        }, {
          name: '时尚',
          id: 'bili_fashion'
        }, {
          name: '广告',
          id: 'bili_ad'
        }, {
          name: '娱乐',
          id: 'bili_ent'
        }, {
          name: '电影',
          id: 'bili_movie'
        }, {
          name: 'TV剧',
          id: 'bili_teleplay'
        }, {
          name: '影视',
          id: 'bili_cinephile'
        }, {
          name: '纪录片',
          id: 'bili_documentary'
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
    }
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
