<template>
  <b-container class="MDContainer">
    <b-row   class="mb-5 mt-5 MD_contAllTT_canal"  align-h="left">
      <div class="TT_indentificador_canal"></div>
      <b-col cols="8" class="MD_contTT_canal">
        <h2>Administrador de Canales de Windows Channel</h2>
      </b-col>
    </b-row>

    <!-- Reproductor y info -->
    <div class="MD_cont_alterAll">
      <alternadora sala="Alternadora" tv=""/>
    </div>
    <!-- <b-row align-h="around" >

      <b-col cols="7">
      </b-col>


    </b-row> -->

    <b-col cols="5" class="channels MD_contAll_video">
        <div>
          <h4 class="text-center MD_contTT_canales">CANALES</h4>
        </div>

      <div class="MD_contVideos">
        <b-button class="mt-2 pl-3 MD_videos_TT" pill variant="success">
          <b-icon class="mr-1" icon="circle-fill" animation="throb" font-scale="1.1"></b-icon>Imbanaco tv 
        </b-button>

        <videoplayerwchannel :options="videoOptions"/>
      </div >

      <div class="MD_contVideos">
        <b-button disabled  class="mt-2 pl-3 MD_videos_TT" pill variant="danger">
          <b-icon class="mr-1 " icon="circle-fill"  font-scale="1.1"></b-icon>Caracol
        </b-button>

        <videoplayerwcomercial :options="videoOptionsComercial"/>
      </div>
    </b-col>
    
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
        width:273,
        height:154, 
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
        width:273,
        height:154, 
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

let $body = document.querySelector("body");

$body.style.backgroundColor = "#F4F8F8";
</script>

<style  scoped>
*{
  font-family: Calibri,Candara,Segoe,Segoe UI,Optima,Arial,sans-serif; 
}


/* #fafafa */
.channels{
    background-color:#fafafa;
    /* E03E52 rojo */
}

/********* CANALES *********/
.MDContainer{
  position: relative;
  min-height: 100vh;
}

div.MD_contAllTT_canal {
  margin-top: 34px !important;
  margin-bottom: 30px !important;
  align-items: center;
}

.TT_indentificador_canal{
  background: #00B2A9;
  height: 25px;
  width: 7px;
  border-radius: 1px;
}

.MD_contTT_canal{
  padding: 0px;
}

.MD_contTT_canal h2 {
    margin-left: 8px;
    font-weight: 700;
    font-size: 35px;
}

.MD_contAll_video {
  position: absolute;
  width: 29.4%;
  background: #ffffff;
  margin-top: 176px;
  /* margin-left: auto; */
  margin-right: 56px;
  padding: 0;
  border-radius: 16px;
  overflow: hidden;
  padding-bottom: 8px;
  box-shadow: 0px 8px 22px 0px rgb(194 198 207 / 50%);
  top: 0;
  right: 0;
}

.MD_contVideos div, .MD_contVideos {
  display: flex;
  justify-content: center;
  flex-direction: column;
  margin: 0 auto;
  margin-bottom: 7px;
}

.MD_videos_TT{
  border-radius: 0!important;
  max-width: 273px;
  margin: 0 auto;
  width: 100%;
  text-align: left;
  padding: 0;
}

.MD_contTT_canales{
  font-size: 18px!important;
  font-weight: 700;
  letter-spacing: 3px;
  color: #ffffff;
  background: #00B2A9;
  padding: 17px 0px;
  margin-bottom: 20px;
}
/********* EndCANALES *********/

</style>
