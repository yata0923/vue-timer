<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <hr />
  現時刻のはず<br />
  <input type="text" :value="now" /><br />
  <hr />
  タイマー (秒)<br />
  <input type="number" v-model="count" /><br />
  <button @click="start()">スタート</button>
  <button @click="stop()">ストップ</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: {},
  data: () => {
    return {
      now: "00:00:00",
      time1: 0 as number,
      count: 5 as number,
      timer: 0 as number,
      audio: {} as HTMLAudioElement,
      started: false as boolean,
    };
  },

  mounted: function () {
    this.time();
    setInterval(this.time, 1000);
    this.audio = new Audio("./alarm.mp3");
    this.audio.loop = true;
  },

  methods: {
    time: function () {
      let date = new Date();
      this.now = date.toLocaleTimeString();
    },

    start: function () {
      if (this.count > 0 && this.started === false) {
        this.timer = setInterval(this.countDown, 1000);
        this.audio.currentTime = 0;
        this.started = true;
      }
    },

    stop: function () {
      clearInterval(this.timer);
      this.audio.pause();
      this.started = false;
      console.log("Stop");
    },

    countDown: function () {
      this.count--;
      if (this.count === 0) {
        clearInterval(this.timer);
        this.audio.play();
        this.started = false;
      }
    },
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
