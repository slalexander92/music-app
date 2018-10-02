<template>
  <div id="app">

  </div>
</template>

<script>
import AudioPlayer from './components/AudioPlayer.vue';
import Controller from './components/Controller.vue';

export default {
  name: 'app',
  components: {
    AudioPlayer,
    Controller
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
