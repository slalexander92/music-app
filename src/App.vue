<template>

  <div id="app"
    v-if="currentTrack"
    :style="{ 'background': 'url(' + currentTrack.cover_image + ')' + ' no-repeat' + ' center' + ' center' + ' fixed' }">

    <div id="info-bar" class="flex-row">
      <AudioPlayer
        :track="currentTrack"
        :artist="artist"
        v-on:change-song="changeSong"/>
    </div>

  </div>

</template>

<script>
import AudioPlayer from './components/AudioPlayer.vue';

export default {
  name: 'app',
  components: {
    AudioPlayer,
  },
  beforeCreate(){

    fetch("https://s3-us-west-1.amazonaws.com/fbx-fed-homework/fed_home_assignment_api.json")
      .then( result => result.json() )
      .then( result => this.album = result )
      .catch( err => console.log( err ) );

  },

  data:() => ({
    album: null,
    album_name: null,
    artist : null,
    tracks: null,
    trackId : 0,
    currentTrack : null,
  }),

  watch: {
    album( album ){
      Object.assign( this, album );
    },
    tracks( tracks ){
      this.currentTrack = tracks[this.trackId];

      console.log( this.currentTrack );
    },
    trackId( id ){
      this.currentTrack = this.tracks[id];
    }
  },

  methods:{
    changeSong( direction ){

      switch( direction ){
        case 'next' :
          if( this.trackId !== this.tracks.length - 1 ){
            this.trackId += 1;
          }
          else{
            this.trackId = 0;
          }
          break;
        case 'prev' :
          if( this.trackId !== 0 ){
            this.trackId -= 1;
          }
          else{
            this.trackId = this.tracks.length - 1;
          }
          break;
      }
    },

  }


};

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
  box-shadow:0 -20px 60px 50px white;

}
</style>
