<template>
  <div id="container">
    <div>番茄钟工作法工作10s休息5s工作或休息结束是可选择是否继续工作或学习</div>
    <div>
      <button @click="startWork">
        开始工作
      </button>
      <button>开始休息</button>
    </div>
    <div>
      <h1 ref="timer" />
    </div>
  </div>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
// eslint-disable-next-line no-undef
const ipcRenderer = require('electron').ipcRenderer;
// eslint-disable-next-line no-undef
const Timer = require('timer.js');


export default defineComponent({
  name: 'App',
  methods: {
    startWork() {
      let workTimer = new Timer({
        tick: 1,
        ontick: (ms: any) => {
          (<any>this.$refs['timer']).innerText = ms;
        },
        onend() {
          ipcRenderer.invoke('work-stop', {}).then(res => {
            alert(res);
          });
        },
      });
      workTimer.start(10);
    },
  },
});
</script>

<style>
</style>
