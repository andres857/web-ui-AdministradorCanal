<template>
  <b-container>
  
    <b-list-group>
        <b-div class="MD_contAll_info_canal">
          <b-container >
            <b-row align-h="start" class="MD_cont_info_canal">

              <b-div class="MD_cont_circleColor">
                <b-icon font-scale=".8" icon="circle-fill" :variant="background"></b-icon>
              </b-div>

              <b-col class="MD_cont_adminCanales">
                <b-icon font-scale="1.1" icon="display"></b-icon>
                <div class="text-center"> Administrador de Canales </div> 
              </b-col>

              <b-div class="MD_line"></b-div>

              <b-col>
                <span class="text-center"> <b> Canal actual: </b> </span>
                <span class="text-center"> {{receiveNews.currentStreaming}} </span>
              </b-col>
              <b-div class="MD_line"></b-div>      
            </b-row>
          </b-container>
        </b-div>
    </b-list-group>       

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

        <b-col cols="8" class="MD_cont_btnReiniciar">
          <b-button size="sm" variant="danger" class="mt-3" @click="doRestartAll(payloads.restartAllPlayers)"> Reiniciar Todos Los Aplicativos </b-button>
          <b-button size="sm" variant="danger" class="mt-3" @click="doRestartAll(payloads.restartAllDevices)"> Reiniciar Todos Los Reproductores </b-button>
        </b-col>

    </div>

  </b-container>
</template>


<script>
const mqtt = require('mqtt')

export default {
  props:[
    'sala',
  ],

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

methods: {

    dochangeStreaming(channel){
      this.client.publish(this.topics.publish.channel, channel, this.qos, error => {
        if (error) {
          console.log('Publish error', error)
        }
        console.log('Publish in the topic', this.topics.publish.channel, channel)
      })
    },

    doRestartAll(target){
      this.client.publish(this.topics.publish.restart, target, this.qos, error => {
        if (error) {
          console.log('Publish error', error)
        }
        console.log('Publish in the topic', this.topics.publish.restart)
      })
    },
},

  mounted:function(){
      
      const { host, port, endpoint} = this.connection
      const connectUrl = `wss://${host}:${port}${endpoint}`

      try {
        this.client = mqtt.connect(connectUrl,this.options)
           } catch (error) {
        console.log('mqtt connect error', error)
       }

      this.client.on('connect', () => {
        console.log('Connection success!')

        this.client.subscribe(this.topics.subscriber.status,  this.qos , (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        console.log('Subscribe to topics res', res)
        })

        this.client.subscribe(this.topics.subscriber.currentStreaming,  this.qos , (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        console.log('Subscribe to topics res', res)
        })

        this.client.publish(this.topics.publish.request, this.payloads.getStatus, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', this.topics.publish.request)
 
        })
        
      })

      this.client.on('error', error => {
        console.log('Connection failed', error)
      })

      this.client.on('message', (topic, message) => {
        // this.receiveNews = this.receiveNews.concat(message)
        console.log(`Received message ${message} from topic ${topic}`)
        this.receiveNews = { ...this.receiveNews , ...JSON.parse(message)};

        if (this.receiveNews.status === 'connected') {
             this.background = 'success'
        }else{
          this.background = 'danger'
        }
        console.log(this.receiveNews)
      })
  }
}
</script>


<style  scoped>

.MD_contAll_info_canal{
  padding: 5px;
  background: #ffffff;
  min-width: 96%;
  margin: 0 auto;
  border-radius: 5px;
  margin-bottom: 54px;
}

.MD_cont_info_canal {
  align-items: center;
}

.MD_cont_circleColor {
  background: #F4F8F8;
  height: 50px;
  width: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  margin-right: 13px;
}

.MD_cont_adminCanales {
  display: flex;
  align-items: center;
}

.MD_cont_adminCanales svg {
  margin-right: 6px;
  fill: #00B2A9;
}

.MD_cont_adminCanales div {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #00B2A9;
}

.MD_cont_info_canal .col, .MD_cont_LogoCanales_canal div .col{
  flex-basis: initial;
  flex-grow: initial;
  max-width: initial;
  width: auto;
  padding: 0;
  text-align: center;
}

.MD_cont_info_canal .col span{
  font-size: 0.9375rem;
}

.MD_cont_info_canal .col:nth-child(4){
  min-width: 20.6%;
}

.MD_cont_info_canal .col:nth-child(6){
  min-width: 13.9%;
}

.MD_cont_info_canal .col:nth-child(8){
  min-width: 15.2%;
}

.MD_cont_info_canal .col:nth-child(10){
  min-width: 7.4%;
  margin-right: 10px;
}

.MD_line {
  background: #908EA3;
  height: 20px;
  width: 1px;
  margin: 0 15px;
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

.MD_cont_LogoCanales_canal div {
 justify-content: space-evenly!important;
}

.MD_cont_allLogosCanales_canal {
  width: 66%;
  padding: 0 32px;
}

.MD_cont_btnReiniciar {
  max-width: 100%;
  text-align: center;
}

.MD_cont_btnReiniciar button {
 margin: 0 11px;
  font-size: 15px;
  font-weight: 700;
  padding: 10px 15px;
  border-radius: 5px;
}
</style>