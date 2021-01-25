<template>
  <div class="home" :style="{'background-image': `url(${backgroundImage})`}">
    <div class="winners">
      <div v-for="(name, index) in select" :key="index">{{ name }}</div>
    </div>
    <div>
      <div @click="onSelectClick" class="button" :style="{'background-image': `url(${buttonImage})`, 'font-size': `${buttonFontSize}px`}">{{buttonText}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      select: ['???', '???'],
      count: 0,
      repeat: false,
      backgroundImage: '',
      buttonText: '开始',
      buttonImage: '',
      buttonFontSize: 16,
      names: '',
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
    start() {
      if (!this.interval) {
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
    const config = JSON.parse(localStorage.getItem('config')) || {};
    console.log(config);
    Object.assign(this.$data, config);
    window.addEventListener('storage', () => {
      // When local storage changes, dump the list to
      // the console.
      const config = JSON.parse(localStorage.getItem('config')) || {};
      Object.assign(this.$data, config);
    });
  },
};
</script>
<style scoped>
.home {
  width: 100vw;
  height: 100vh;
  background-size: cover;
}
.winners {
  display: flex;
}
.winners > div {
  margin: 10px;
}
.button {
  position: absolute;
  background-size: cover;
}
</style>
