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
          <p id="artist">{{ artist }}</p>
          <p>{{ track.name }}</p>
        </div>
        <div id="control-container" class="flex-column">
          <div class="progress-bar">
            <div class="indicator" :style="{ width : indicatorWidth + '%' }"></div>
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

    // duration
    this.audioTrack.addEventListener('loadedmetadata', ( e ) => {
      const duration = this.audioTrack.duration;
      const mins     = parseInt( duration / 60, 10 );
      const seconds  = parseInt( duration % 60, 10 );

      seconds < 10 ? this.duration = `${mins}:0${seconds}` : this.duration = `${mins}:${seconds}`

    });

    // current time
    this.audioTrack.addEventListener('timeupdate', () => {
        let s = parseInt(this.audioTrack.currentTime % 60);
        let m = parseInt((this.audioTrack.currentTime / 60) % 60);

        s < 10 ? this.elapsed = `${m}:0${s}` : this.elapsed = `${m}:${s}`

        this.indicatorWidth = ( this.audioTrack.currentTime / this.audioTrack.duration ) * 100;
    });

  },
  props:{
    artist: String,
    track: Object,
  },
  data:() => ({
    state:'play',
    audioTrack : '',
    trackURL: '',
    duration:'',
    elapsed : '',
    indicatorWidth: 0,
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


#audio-player{
  margin: 0 auto;
}

#controller{
  margin-right: 30px;
}

#player{

  #artist{
    margin-right: 30px;
  }
}

#control-container{
  //TODO: fill in progress bar by percentage based on elapsed / duration

  .progress-bar{
    width: 600px;
    height: 3px;
    border-radius: 2px;
    background-color: $gray;
    margin: 10px 0;

    .indicator{
      background-color: $green;
      height: 100%;
      transition: all .1s ease;
    }
  }

  #duration{
    justify-content: space-between;
    p{
      color: #adadad
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
