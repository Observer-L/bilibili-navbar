<template>
  <div id="app">
    <div class="header"></div>
    <Floor :floors="floors"></Floor>
    <NavBar :options="options"></NavBar>
    <div class="footer"></div>
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
</style>
