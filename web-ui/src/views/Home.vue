<template>
  <div >
   <b-container>
     <b-row class="mt-5">
       <b-col lg="4">
       <player1/>
       <h1></h1>
       <player2/>
       <player3/>
       </b-col>

       <b-col lg="8">
          <videoplayer :options="videoOptions" />
       </b-col>

     </b-row>
   </b-container>
  </div>
</template>


<script>
import videoplayer from '../components/videoplayer.vue'
import player1 from '../components/rpi1.vue'
import player2 from '../components/rpi2.vue'
import player3 from '../components/rpi3.vue'

import 'video.js/dist/video-js.css'

export default {
  components: {
    videoplayer,
    player1,
    player2,
    player3,
  },
  data:function(){
        return{
              videoOptions: { 
                autoplay: false, 
                controls: true,
                width:690,
                height:390, 
                sources: [ 
                  { 
                    src:"http://165.227.2.248:8091/hls/lilitv.m3u8",
                     type: "application/x-mpegURL" } ], 
                  },
        }    
  },
}
</script>
