<template>
  <b-container>
    <b-row class="mb-5 mt-2">
      <b-col>
        <h1>Administrador de canales</h1>
      </b-col>
    </b-row>
    <!-- Reproductor y info -->
    <b-row>
      <b-col>
        <alternadora sala="Alternadora" tv=""/>
      </b-col>
    </b-row>
    <b-row align-h="center" >
      <b-col cols="8">
        <videoplayer :options="videoOptions"/>
      </b-col>
    </b-row>
    
  </b-container>
</template>


<script>
import videoplayer from '../components/videoplayer.vue'
import alternadora from '../components/alternadora.vue'
import 'video.js/dist/video-js.css'

export default {
  components: {
    alternadora,
    videoplayer,
  },
  data(){
    return{
      videoOptions: { 
        autoplay: false, 
        controls: true,
        width:690,
        height:390, 
        sources: [ 
          { 
            src:"https://rtmpetb.windowschannel.us/hls/streaming.m3u8",
            // src: "https://rtmpimbanaco.windowschannel.us/hls/streaming.m3u8",
            type: "application/x-mpegURL" } ], 
          },
    }
  },
  methods:{
    
  },
  
}
</script>

<style  scoped>

</style>
