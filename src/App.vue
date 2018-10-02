<template>
  <div id="app">

    <div id="info-bar" class="flex-row">
      <Controller />
      <AudioPlayer />
    </div>

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
  },
  data: () => ({

  })
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  width: 100%;
}

#info-bar{
  background-color: white;
  position: absolute;
  width: 100%;
  bottom: 0;
  height: 20%;
  padding: 15px 30px;
}
</style>
