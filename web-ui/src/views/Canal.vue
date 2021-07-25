<template>
  <b-container>
    <b-row   class="mb-5 mt-5"  align-h="center">
      <b-col cols="8">
        <h2>Administrador de Canales de Windows Channel</h2>
      </b-col>
    </b-row>

    <!-- Reproductor y info -->
    <b-row align-h="around" >

      <b-col cols="7">
        <alternadora sala="Alternadora" tv=""/>
      </b-col>

       <b-col cols="5" class="channels ">
         <div>
           <h4 class="m-3 text-center">Canales</h4>
         </div>

        <div class="ml-4">
          <videoplayerwchannel :options="videoOptions"/>

          <b-button class="mt-2 pl-3" pill variant="success">Imbanaco tv 
            <b-icon class="ml-1" icon="circle-fill" animation="throb" font-scale="1.1"></b-icon>
          </b-button>

        </div >
        <div class="mt-3 ml-4">
          <videoplayerwcomercial :options="videoOptionsComercial"/>

          <b-button disabled  class="mt-2 pl-3" pill variant="danger">Caracol
            <b-icon class="ml-1 " icon="circle-fill"  font-scale="1.1"></b-icon>
          </b-button>

        </div>
      </b-col>

    </b-row>

    
  </b-container>
</template>


<script>
import videoplayerwchannel from '../components/videoplayerwchannel.vue'
import videoplayerwcomercial from '../components/videoplayercomercial.vue'
import alternadora from '../components/alternadora.vue'
import 'video.js/dist/video-js.css'

export default {
  components: {
    alternadora,
    videoplayerwchannel,
    videoplayerwcomercial
  },

  data(){
    return{
      videoOptions: { 
        autoplay: false, 
        controls: true,
        width:380,
        height:214, 
        poster:'https://mediawebwordpress.nyc3.cdn.digitaloceanspaces.com/imbanaco/bgwc.png',
        sources: [ 
          { 
            src:"https://rtmpetb.windowschannel.us/hls/streaming.m3u8",
            type: "application/x-mpegURL" }
          ], 
      },

      videoOptionsComercial: { 
        autoplay: false, 
        controls: true,
        width:380,
        height:214, 
        poster:'https://mediawebwordpress.nyc3.cdn.digitaloceanspaces.com/imbanaco/bgcomercial.png',
        sources: [ 
          { 
            src: "https://rtmpimbanaco.windowschannel.us/hls/streaming.m3u8",
            type: "application/x-mpegURL" }
          ], 
      },


    }
  },
}
</script>

<style  scoped>
/* #fafafa */
.channels{
    background-color:#fafafa;
    /* E03E52 rojo */
}

</style>
