<template>

  <div id="audio-player">
    <div class="flex-row">
      <div id="controller">
        <i class="material-icons" @click="prevSong">skip_previous</i>
        <i class="material-icons" v-show="trackState.state === 'pause' " @click=" trackState.state ='play' ">play_arrow</i>
        <i class="material-icons" v-show="trackState.state === 'play' " @click=" trackState.state = 'pause' ">pause</i>
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
            <!-- <p>{{ trackState.elapsed }}</p> -->
            <p>{{ trackState.duration }}</p>
          </div>
        </div>
        <audio id='audio-track' ref="audio" :src='track.url' preload="metadata" autoplay></audio>
      </div>
    </div>
  </div>

</template>

<script>
export default {

  mounted(){

    const audio = document.getElementById('audio-track');

    audio.addEventListener("loadedmetadata", ( e ) => {
      const duration = audio.duration;
      const mins     = parseInt( audio.duration / 60, 10 );
      const seconds  = parseInt( audio.duration % 60, 10 );

      this.trackState.duration = `${mins}:${seconds}`
    });
  },
  props:{
    artist: String,
    track: Object,
  },
  data:() => ({
    trackState:{
      state:'play',
      elapsed:'',
      duration:'',
    }
  }),
  watch:{
    track( track ){

      this.trackState = {
        state:'play',
        elapsed:'',
        duration:'',
      }

      this.trackURL = track.url;
    },

    'trackState.state':function( state ){

      if( state === 'play' ){
        this.$refs.audio.play();
      }
      else{
        this.$refs.audio.pause();
      }
    },

  },
  computed:{

    timeElapsed(){
      //TODO
    },

  },
  methods:{
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

#control-container{
  //TODO: fill in progress bar by percentage based on elapsed / duration
  .progress-bar{

    .indicator{
      //
    }

  }

}


i.material-icons{
  cursor: pointer;
  color: $green;
  font-size: 61px;

  &:hover{
    color:$hoverGreen;
  }
}

</style>
