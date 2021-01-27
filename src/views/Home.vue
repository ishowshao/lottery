<template>
  <div class="home" :style="{'background-image': `url(${backgroundImage})`}">
    <div class="winner" :style="{'font-size': `${winnerFontSize}px`, color: `${winnerColor}`, transform: `translate(${winnerTranslateX}px,${winnerTranslateY}px)`}">
      <div v-if="init">{{initText}}</div>
      <div v-else v-for="(name, index) in select" :key="index">{{ name }}</div>
    </div>
    <div @click="onSelectClick" class="button" :style="{'background-image': `url(${buttonImage})`, 'font-size': `${buttonFontSize}px`, transform: `translate(${buttonTranslateX}px,${buttonTranslateY}px)`, width: `${buttonWidth}px`, height: `${buttonHeight}px`, 'line-height': `${buttonHeight}px`}">{{buttonText}}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      select: [],
      init: true,

      count: 1,
      names: '',
      chosen: '',
      repeat: false,
      initText: '',
      backgroundImage: '',
      buttonText: '开始',
      buttonText2: '结束',
      buttonImage: '',
      buttonFontSize: 16,
      buttonColor: '#000000',
      buttonWidth: 80,
      buttonHeight: 60,
      buttonTranslateX: 0,
      buttonTranslateY: 0,
      winnerFontSize: 40,
      winnerColor: '#000000',
      winnerTranslateX: 0,
      winnerTranslateY: 0,

      interval: null,
    };
  },
  methods: {
    randomSelect() {
      const names = this.names.split(',');
      const result = [];
      if (this.count > names.length) {
        alert('目标中奖人数大于名单人数');
        return;
      }
      while (result.length < this.count) {
        const select = names[Math.floor(Math.random() * names.length)];
        if (result.indexOf(select) === -1) {
          result.push(select);
        }
      }
      return result;
    },
    resetAction() {
      const config = JSON.parse(localStorage.getItem('config'));
      config.action = null;
      localStorage.setItem('config', JSON.stringify(config));
    },
    start() {
      if (!this.interval && this.init) {
        this.init = false;
        this.select = this.randomSelect();
        this.interval = setInterval(() => {
          this.select = this.randomSelect();
        }, 100);
      }
    },
    stop() {
      if (this.interval) {
        clearInterval(this.interval);
        this.interval = null;
      }
    },
    onSelectClick() {
      if (!this.interval) {
        this.start();
      } else {
        this.stop();
      }
    }
  },
  mounted() {
    let config = {};
    if (localStorage.getItem('config')) {
      config = JSON.parse(localStorage.getItem('config'));
    }
    console.log(config);
    Object.assign(this.$data, config);
    window.addEventListener('storage', () => {
      console.log('receive message');
      const config = JSON.parse(localStorage.getItem('config'));
      Object.assign(this.$data, config);

      const action = config.action;
      console.log('action', action);
      if (action === 'init') {
        this.init = true;
      } else if (action === 'start') {
        this.start();
      } else if (action === 'stop') {
        this.stop();
      }
      this.resetAction();
    });
  },
};
</script>
<style scoped>
.home {
  display: flex;
  width: 100vw;
  height: 100vh;
  background-size: cover;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.winner {
  display: flex;
}
.winner > div {
  margin: 10px;
}
.button {
  background-repeat: no-repeat;
  background-size: cover;
  overflow: hidden;
  text-align: center;
  cursor: pointer;
}
</style>
