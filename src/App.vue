<template>
  <div id="app">
    <!--    <nav>-->
    <!--      <router-link to="/">Home</router-link> |-->
    <!--      <router-link to="/about">About</router-link>-->
    <!--    </nav>-->
    <!--    <router-view />-->
    <div class="info">{{ _info }}</div>
    <audio controls class="audioPlayer" :ref="playerId">
      <source :src="audioSource" type="audio/mp3" />
      Your browser does not support the <code>audio</code> element.
    </audio>
    <img
      src="@/assets/frog.png"
      class="btn"
      :style="imgStyle"
      @click="handlePlay"
    />
    <div class="version">Version: {{ version }}</div>
  </div>
</template>
<script>
import frog from "@/assets/audio/frog_1.mp3";
import AppInfo from "../package.json";

console.log(AppInfo);
let audioPlayer = null;
export default {
  name: "App-Entry",
  data() {
    return {
      version: AppInfo.version,
      window: {
        innerWidth: 0,
        innerHeight: 0,
      },
      audioSource: frog,
      playerId: "player",
      playing: false,
      vibrateData: {
        // 是否可用
        canUse: false,
        //参数
        // 震动200ms
        parameters: 500,
        // 震动  'SOS' in Morse.
        // parameters: [
        //   100, 30, 100, 30, 100, 30, 200, 30, 200, 30, 200, 30, 100, 30, 100,
        //   30, 100,
        // ],
      },
      err: false,
    };
  },
  computed: {
    imgStyle() {
      return { width: `${innerWidth / 4}px` };
    },
    _info() {
      const { err, vibrateData } = this;
      let ret = { vibrateData };
      if (err) {
        Object.assign(ret, { err });
      }
      return ret;
    },
  },
  created() {
    navigator.vibrate =
      navigator.vibrate ||
      navigator.webkitVibrate ||
      navigator.mozVibrate ||
      navigator.msVibrate;
    if (navigator.vibrate) {
      this.vibrateData.canUse = true;
    } else {
      this.vibrateData.canUse = false;
    }
  },
  methods: {
    handlePlay() {
      // if (this.playing) return;
      const { vibrateData } = this;
      if (audioPlayer) {
        try {
          audioPlayer.play();
          if (vibrateData.canUse) {
            navigator.vibrate(vibrateData.parameters);
          }
        } catch (err) {
          this._info = err;
        }
      }
    },
    bindWindowSize() {
      const { innerWidth, innerHeight } = window;
      console.log(innerWidth, innerHeight);
      Object.assign(this.window, { innerWidth, innerHeight });
    },
    logWindow() {
      // 获取浏览器窗口可视区域的宽度  不包含滚动条
      const { clientHeight, clientWidth } = document.body;
      console.log(clientWidth, clientHeight);

      // 获取浏览器窗口可视区域的宽度 包含滚动条
      // 获取浏览器窗口可视区域的高度
      const { innerWidth, innerHeight } = window;
      console.log(innerWidth, innerHeight);

      // 获取浏览器窗口可视区域的宽度  不包含滚动条
      const {
        clientWidth: documentElementClientWidth,
        clientHeight: documentElementClientHeight,
      } = document.documentElement;
      console.log(documentElementClientWidth, documentElementClientHeight);
    },
  },
  mounted() {
    this.bindWindowSize();
    audioPlayer = this.$refs[this.playerId];
    window.addEventListener("resize", () => {
      this.bindWindowSize();
    });
  },
};
</script>
<style scoped lang="scss">
#app {
  //background-color: #42b983;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;

  .info {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    background-color: #fff;
    padding: 10px;
  }

  .audioPlayer {
    visibility: hidden;
    width: 0;
    height: 0;
  }

  .btn {
    //height: 50px;
    //width: 50px;
    background-color: rgba(0, 0, 0, 0);
    border-radius: 50%;
    cursor: pointer;
    user-selcet: none;
    -webkit-tap-highlight-color: transparent;
  }
  .version {
    position: absolute;
    bottom: 0;
    color: #fff;
    font-size: 14px;
    font-weight: 100;
    padding: 10px;
  }
}
</style>
<!--<style lang="scss">-->
<!--#app {-->
<!--  font-family: Avenir, Helvetica, Arial, sans-serif;-->
<!--  -webkit-font-smoothing: antialiased;-->
<!--  -moz-osx-font-smoothing: grayscale;-->
<!--  text-align: center;-->
<!--  color: #2c3e50;-->
<!--}-->

<!--nav {-->
<!--  padding: 30px;-->

<!--  a {-->
<!--    font-weight: bold;-->
<!--    color: #2c3e50;-->

<!--    &.router-link-exact-active {-->
<!--      color: #42b983;-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</style>-->
