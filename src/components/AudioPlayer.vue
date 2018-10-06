<template>

  <div id="audio-player">
    <div class="flex-row">
      <div id="controller">
        <i class="material-icons"
          @click="prevSong">skip_previous</i>
        <i class="material-icons"
          v-show="state === 'pause' "
          @click=" state ='play' ">play_arrow</i>
        <i class="material-icons"
          v-show="state === 'play' "
          @click=" state = 'pause' ">pause</i>
        <i class="material-icons"
          @click="nextSong">skip_next</i>
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
          <div id="duration" class="flex-row">
            <p>{{ elapsed }}</p>
            <p>{{ duration }}</p>
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

    this.audioTrack = document.getElementById('audio-track');

    this.audioTrack.addEventListener('loadedmetadata', ( e ) => {
      const duration = this.audioTrack.duration;
      const mins     = parseInt( duration / 60, 10 );
      const seconds  = parseInt( duration % 60, 10 );

      this.duration = `${mins}:${seconds}`;
    });

    // Countdown
    this.audioTrack.addEventListener('timeupdate', () => {
        let s = parseInt(this.audioTrack.currentTime % 60);
        let m = parseInt((this.audioTrack.currentTime / 60) % 60);

        s < 10 ? this.elapsed = `${m}:0${s}` : this.elapsed = `${m}:${s}`

    }, false);

  },
  props:{
    artist: String,
    track: Object,
  },
  data:() => ({
    state:'play',
    audioTrack : '',
    duration:'',
    trackURL: '',
    elapsed : '',
  }),
  watch:{
    track( track ){

      this.state = 'play';
      this.trackURL = track.url;
    },
    state( state ){

      if( state === 'play' ){
        this.audioTrack.play();
      }
      else{
        this.audioTrack.pause();
      }
    },

  },
  computed:{

    // elapsed(){
    //   return parseInt( this.audioTrack.currentTime, 10);
    // },

  },
  methods:{
    nextSong(){
      this.$emit('change-song', 'next');
      this.audioTrack.play();
    },
    prevSong(){
      this.$emit('change-song', 'prev');
      this.audioTrack.pause();
    }

  },

}
</script>

<style lang="scss" scoped>

@import '../globals/variables';

#control-container{
  //TODO: fill in progress bar by percentage based on elapsed / duration

  .progress-bar{
    width: 400px;
    height: 3px;
    border-radius: 2px;
    background-color: $gray;
    margin: 5px 0;

    .indicator{
      //
    }
  }

  #duration{
    justify-content: space-between;
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
