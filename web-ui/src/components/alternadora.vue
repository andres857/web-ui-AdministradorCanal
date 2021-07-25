<template>
  <b-container>
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-4 class="d-flex justify-content-between align-items-center">
          
          <b-container >
            <b-row align-h="start">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon icon="display"></b-icon>
              </b-col>

              <b-col cols="5">
                <span class="text-center"> Administrador de Canales </span> 
              </b-col>

              <b-col cols="5">
                <span class="text-center"> <b> Canal actual </b> </span> |
                <span class="text-center"> {{receiveNews.currentStreaming}} </span>
              </b-col>
      
            </b-row>

          </b-container>
            

        </b-list-group-item>

          <b-collapse id="collapse-4" class="mb-3" >
            <b-card>
                <b-container>
                  <b-row >

                    <b-col cols="6" >
                        <div class="mt-1">
                          <span><strong>IPv4</strong></span>
                          <span> {{receiveNews.ip4}}</span>
                          <br>
                          <span><strong>MAC</strong></span>
                          <span> {{receiveNews.MAC}}</span> 
                        </div>           
                    </b-col>

                    <b-col cols="6">
                        <b-col>
                            <b-icon icon="thermometer" font-scale="1.5"></b-icon>
                            {{receiveNews.main}} °C 
                        </b-col>
                        <b-col class="mt-2">
                            <b-icon icon="cpu" font-scale="1.5"></b-icon>
                            <span> {{receiveNews.currentLoad}}%  </span>
                        </b-col>
                    </b-col>
                </b-row>

                </b-container>
            </b-card>
          </b-collapse>
    </b-list-group>       

    <b-row align-h="around" style="text-align:center">   

      <b-col cols='3' class="p-2" >
          <b-img width="250" height="150" :src="imbanaco" style="cursor: pointer" @click="dochangeStreaming(channels.wchannel)" ></b-img>
      </b-col>  

      <b-col cols='2' class="p-2" >
        <b-img width="150" height="150" :src="rcn" style="cursor: pointer" @click="dochangeStreaming(channels.rcn)"></b-img>

      </b-col>

    </b-row>   

    <b-row align-h="around" style="text-align:center">   

      <b-col cols='2' class="p-2" >
        <b-img width="100" height="100" :src="tnt" style="cursor: pointer" @click="dochangeStreaming(channels.tnt)" ></b-img>
      </b-col>

      <b-col cols='2' class="p-3"  >
        <b-img width="120" height="120" :src="caracol" style="cursor: pointer" @click="dochangeStreaming(channels.caracol)" ></b-img>
      </b-col>

    </b-row> 
      <b-col cols="8">
        <b-button size="sm" variant="danger" class="mt-3" @click="doRestartAll(payloads.restartAllPlayers)"> Reiniciar Todos los Player </b-button>
        <b-button size="sm" variant="danger" class="mt-3" @click="doRestartAll(payloads.restartAllDevices)"> Reiniciar Todos los Dispositivos </b-button>
      </b-col>

    <b-row>
      <b-button class="mt-2 pl-3" pill :variant=backgroundChannelwc >Imbanaco tv 
            <b-icon class="ml-1" icon="circle-fill" :animation=animationChannelwc font-scale="1.1"></b-icon>
      </b-button>

      <b-button disabled  class="mt-2 pl-3" pill variant="danger">Caracol
            <b-icon class="ml-1 " icon="circle-fill"  font-scale="1.1"></b-icon>
      </b-button>
    </b-row>



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
      caracol: require('../assets/Caracol.svg'),
      tnt: require('../assets/tnt.svg'),
      rcn: require('../assets/rcn.svg'),
      imbanaco: require('../assets/imbanaco.svg'),

        backgroundChannelwc:'success',
        animationChannelwc:'throb',
      

      status:'',
      background:'danger',
      connection: {
        host: 'broker.windowschannel.us',
        port: 8083,
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
        wchannel: '{"channel":"imbanacotv"}',
        caracol: '{"channel":"caracol"}',
        rcn : '{"channel":"rcn"}',
        tnt: '{"channel":"scolombia"}',
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

  methods: {

      dochangeStreaming(channel){
        this.client.publish(this.topics.publish.channel, channel, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.channel,channel)
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
      const connectUrl = `ws://${host}:${port}${endpoint}`

      try {
        this.client = mqtt.connect(connectUrl,this.options)
           } catch (error) {
        console.log('mqtt.connect error', error)
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
        this.receiveNews = JSON.parse(message);
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