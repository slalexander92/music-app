<template>

  <div id="audio-player">
    <div class="flex-row">
      <div id="controller">
        <i class="material-icons" @click="prevSong">skip_previous</i>
        <i class="material-icons" v-show="trackState.state === 'pause' " @click="toggleState( 'play' )">play_arrow</i>
        <i class="material-icons" v-show="trackState.state === 'play' " @click="toggleState( 'pause' )">pause</i>
        <i class="material-icons" @click="nextSong">skip_next</i>
      </div>
      <div id="player">
        <div class="flex-row">
          <p>{{ artist }}</p>
          <p>{{ track.name }}</p>
        </div>
        <div id="control-container" class="flex-column">
          <div class="progress-bar">
            <div class="indicator"></div>
          </div>
          <div class="flex-row">
            <p></p>
            <p>{{ trackState.duration }}</p>
          </div>
        </div>
        <audio id='audio-track' ref="audio" :src='track.url' preload="metadata" ></audio>
      </div>
    </div>
  </div>

</template>

<script>
export default {

  props:{
    artist: String,
    track: Object,
    trackState : Object,
  },
  watch:{
    track( track ){
      this.trackURL = track.url;
    },
  },
  mounted(){
    const audio = document.getElementById('audio-track');

    audio.addEventListener("loadedmetadata", ( e ) => {
      const duration = audio.duration;
      const mins     = parseInt( audio.duration / 60, 10 );
      const seconds  = parseInt( audio.duration % 60 );

      this.trackState.duration = `${mins}:${seconds}`
    });
  },
  computed(){

  },
  methods:{

    toggleState( changeState ){

      if( changeState === 'play' ){
        this.trackState.state = 'play';
        this.$refs.audio.play();
      }
      else{
        this.trackState.state = 'pause';
        this.$refs.audio.pause();
      }

    },
    nextSong(){
      this.$emit('change-song', 'next');
      this.$refs.audio.play();
    },
    prevSong(){
      this.$emit('change-song', 'prev');
      this.$refs.audio.pause();
    }

  },

}
</script>

<style lang="scss" scoped>

@import '../globals/variables';

i.material-icons{
  cursor: pointer;
  color: $green;
  font-size: 61px;

  &:hover{
    color:$hoverGreen;
  }
}

</style>
