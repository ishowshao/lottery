<template>
  <div class="home" :style="{'background-image': `url(${backgroundImage})`}">
    <div class="winners">
      <div v-for="(name, index) in names" :key="index">{{ name }}</div>
    </div>
    <div>
      <div class="button" :style="{'background-image': `url(${buttonImage})`, 'font-size': `${buttonFontSize}px`}">{{buttonText}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      names: ["???", "???"],
      count: 0,
      repeat: false,
      backgroundImage: '',
      buttonText: '开始',
      buttonImage: '',
      buttonFontSize: 16,
    };
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
.button {
  position: absolute;
  background-size: cover;
}
</style>
