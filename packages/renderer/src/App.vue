<template>
  <div id="container">
    <div>番茄钟工作法工作10s休息5s工作或休息结束是可选择是否继续工作或学习</div>
    <div>
      <button @click="setState({duration :10,isWork :true})">
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

const {ipcRenderer} = require('electron');
const Timer = require('timer.js');

export default defineComponent({
  name: 'App',
  methods: {
    setState(options) {
      // eslint-disable-next-line @typescript-eslint/no-this-alias
      let vm = this;
      let timer = new Timer({
        tick: 1,
        ontick: (ms: string) => {
          let text = `正在${options.isWork ? '工作' : '休息'}..剩余${ms}ms`;
          (<never>this.$refs['timer']).innerText = text;
        },
        async onend() {
          let res = await ipcRenderer.invoke('state-changed', {
            endedState: options.isWork,
          });
          console.log(vm);
          if (res.newState) {
            vm.setState({duration: 10, isWork: true});
          } else {
            vm.setState({duration: 5, isWork: false});
          }
        },
      });
      timer.start(options.duration);
    },
  },
});
</script>

<style>
</style>
