<template>
  <b-container>
    <b-row class="mb-5 mt-2">
      <b-col>
        <h1>Administrador de canales</h1>
      </b-col>
    </b-row>
    <!-- Reproductor y info -->
    <b-row align-v="start">
      <b-col cols="4">
        <b-button @click="alternarCanal" variant="success">Alternar</b-button>
      </b-col>
      <b-col lg="8">
          <b-row align-v="center" class="mb-2">
            <b-col>
              <h3>Emision Actual editarbx</h3>
            </b-col>
            <b-col>
              <h3>{{canalEmision.nombre}}</h3>
            </b-col>
            <b-col>
              <h3>
                {{canalEmision.canal}}<br>
              </h3>
            </b-col>
          </b-row>
          <videoplayer :options="videoOptions" />
      </b-col>
    </b-row>
  </b-container>
</template>



<script>
import videoplayer from '../components/videoplayer.vue'
import 'video.js/dist/video-js.css'

export default {
  components: {
    videoplayer,
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
                    src:"https://rtmpetb.windowschannel.us/hls/streaming.m3u8",
                     type: "application/x-mpegURL" } ], 
                  },

            canalComercial:{
              nombre:'Caracol',
              canal:'145',
              emision:false,
            },
            canalInstitucional:{
              nombre:'Imbanaco TV',
              canal:'',
              emision:true,
            },
            canalEmision:{
              nombre:'Imbanaco TV',
              canal:'',
            }
            
        }
  },
  methods:{
    alternarCanal(){
      let vm = this;
      if(vm.canalInstitucional.emision){
        console.log('canal instiucional alternar')
        vm.canalInstitucional.emision= false;
        vm.canalComercial.emision=true;
        
        if(vm.canalInstitucional.emision){
          console.log('Canal institucional');
          vm.canalEmision.nombre = vm.canalInstitucional.nombre;
          vm.canalEmision.canal = '';

        }else{
          vm.canalEmision.nombre = vm.canalComercial.nombre;
          vm.canalEmision.canal = vm.canalComercial.canal;
        }
    
      }else{
        vm.canalInstitucional.emision= true;
        vm.canalComercial.emision=false;
        if(vm.canalInstitucional.emision){
          console.log('Canal institucional');
          vm.canalEmision.nombre = vm.canalInstitucional.nombre;
          vm.canalEmision.canal = '';

        }else{
          vm.canalEmision.nombre = vm.canalComercial.nombre;
          vm.canalEmision.canal = vm.canalComercial.canal;
        }
      }
        
    }
  },
  mounted: function obtenerelcanalActual(){
      let vm = this;
        if(vm.canalInstitucional.emision){
          console.log('Canal institucional');
          vm.canalEmision.nombre = vm.canalInstitucional.nombre;
          vm.canalEmision.canal = '';

        }else{
          vm.canalEmision.nombre = vm.canalComercial.nombre;
          vm.canalEmision.canal = vm.canalComercial.canal;
        }
    }
  
}
</script>

<style  scoped>

</style>
