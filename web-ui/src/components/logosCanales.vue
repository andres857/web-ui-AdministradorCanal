<template>
    <b-container>
        <div class="MD_cont_allLogosCanales_canal">

      <div class="MD_cont_LogoCanales_canal">
        <b-row align-h="around" style="text-align:center">   

          <b-col class="p-2 MD_cont_canal" >
              <b-img :src="ImbanacoSrc" style="cursor: pointer" @click="dochangeStreaming(channels.wchannel)" ></b-img>
          </b-col>  

          <b-col class="p-2 MD_cont_canal" >
            <b-img :src="rcnSrc" style="cursor: pointer" @click="dochangeStreaming(channels.rcn)"></b-img>
          </b-col> 

          <b-col class="p-2 MD_cont_canal" >
            <b-img :src="tntSrc" style="cursor: pointer" @click="dochangeStreaming(channels.tnt)" ></b-img>
          </b-col>

          <b-col class="p-3 MD_cont_canal"  >
            <b-img :src="caracolSrc" style="cursor: pointer" @click="dochangeStreaming(channels.caracol)" ></b-img>
          </b-col>

        </b-row> 

        <b-row align-h="around" style="text-align:center">   

          <b-col class="p-2 MD_cont_canal" >
              <b-img :src="espnSrc" style="cursor: pointer" @click="dochangeStreaming(channels.wchannel)" ></b-img>
          </b-col>  

          <b-col class="p-2 MD_cont_canal" >
            <b-img :src="historySrc" style="cursor: pointer" @click="dochangeStreaming(channels.rcn)"></b-img>
          </b-col> 

          <b-col class="p-2 MD_cont_canal" >
            <b-img :src="nickSrc" style="cursor: pointer" @click="dochangeStreaming(channels.tnt)" ></b-img>
          </b-col>

          <b-col class="p-3 MD_cont_canal"  >
            <b-img :src="teleantioquiaSrc" style="cursor: pointer" @click="dochangeStreaming(channels.caracol)" ></b-img>
          </b-col>

        </b-row>

      </div>  
    </div>
    </b-container>
</template>

<script>
export default{
    data() {
    return {
        // images
        caracolOff: require('../assets/caracolOff.png'),
        caracolOn: require('../assets/caracolOn.png'),
        tntOff: require('../assets/tntOff.png'),
        tntOn: require('../assets/tntOn.png'),
        rcnOff: require('../assets/rcnOff.png'),
        rcnOn: require('../assets/rcnOn.png'),
        imbanacoOff: require('../assets/imbanacoOff.png'),
        imbanacoOn: require('../assets/imbanacoOn.png'),

        historyOff: require('../assets/historyOff.png'),
        historyOn: require('../assets/historyOff.png'),
        espnOff: require('../assets/espnOff.png'),
        espnOn: require('../assets/espnOn.png'),
        nickOff: require('../assets/nicklOff.png'),
        nickOn: require('../assets/nicklOn.png'),
        teleantioquiaOff: require('../assets/teleantioquiaOff.png'),
        teleantioquiaOn: require('../assets/teleantioquiaOn.png'),

        backgroundChannelwc:'success',
        animationChannelwc:'throb',
        
        status:'',
        background:'danger',


        connection: {
        host: 'brokerimbanaco.windowschannel.com',
        port: 8084,//port web socket
        endpoint: '/mqtt',
        clean: true, // Reserved session
        connectTimeout: 4000, // Time out
        reconnectPeriod: 10000, // Reconnection interval
        
        },
        options:{
            // Certification Information
        clientId: 'webClientAlternadora',
        username: 'emqx',
        password: 'public',
        },

        topics: {
        subscriber:{
            currentStreaming:'imbanaco/principal/currentStreaming',
            status:'imbanaco/principal/alternadora/b5c890/status',
        },
        publish:{
            request: `imbanaco/principal/alternadora/b5c890/request`,
            channel: `imbanaco/principal/players/channel`,
            restart: `imbanaco/principal/players/restart`
        },
        },

        channels:{
        wchannel: '{"channel":"Imbanaco tv"}',
        caracol: '{"channel":"caracol"}',
        rcn : '{"channel":"rcn"}',
        tnt: '{"channel":"tnt"}',
        espn: '{"channel":"espn"}',
        history: '{"channel":"history"}',
        nick: '{"channel":"nick"}',
        teleantioquia: '{"channel":"teleantioquia"}'
        },

        payloads:{
        getStatus:'{ "status": "get" }',
        restartAllPlayers: '{ "restart": "player" }',
        restartAllDevices: '{ "restart": "device" }',
        },
        
        qos: 2,
        receiveNews: '',
        
    }
    },
    computed:{
  ImbanacoSrc(){
    if (this.receiveNews.currentStreaming == 'Imbanaco tv'){
      return this.imbanacoOn
    }else{
      return this.imbanacoOff
    }
  },
  caracolSrc(){
    if (this.receiveNews.currentStreaming == 'caracol'){
      return this.caracolOn
    }else{
      return this.caracolOff
    }
  },
  rcnSrc(){
    if (this.receiveNews.currentStreaming == 'rcn'){
      return this.rcnOn
    }else{
      return this.rcnOff
    }
  },
  tntSrc(){
    if (this.receiveNews.currentStreaming == 'tnt'){
      return this.tntOn
    }else{
      return this.tntOff
    }
  },
  espnSrc(){
    if (this.receiveNews.currentStreaming == 'espn'){
      return this.espnOn
    }else{
      return this.espnOff
    }
  },
  historySrc(){
    if (this.receiveNews.currentStreaming == 'history'){
      return this.historyOn
    }else{
      return this.historyOff
    }
  },
  nickSrc(){
    if (this.receiveNews.currentStreaming == 'nick'){
      return this.nickOn
    }else{
      return this.nickOff
    }
  },
  teleantioquiaSrc(){
    if (this.receiveNews.currentStreaming == 'teleantioquia'){
      return this.teleantioquiaOn
    }else{
      return this.teleantioquiaOff
    }
  },
 
},
}

</script>

<style  scoped>

.MD_cont_allLogosCanales_canal {
  width: 66%;
  padding: 0 32px;
}

.MD_cont_LogoCanales_canal div {
 justify-content: space-evenly!important;
}

.MD_cont_LogoCanales_canal div .MD_cont_canal {
  background: #DFEDEC;
  width: 119px;
  height: 119px;
  padding: 0;
  margin: 0;
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 38px;
}

.MD_cont_info_canal .col, .MD_cont_LogoCanales_canal div .col{
  flex-basis: initial;
  flex-grow: initial;
  max-width: initial;
  width: auto;
  padding: 0;
  text-align: center;
}
</style>

