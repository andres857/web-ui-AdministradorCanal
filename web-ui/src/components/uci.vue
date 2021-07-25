<template>
  <b-container>
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-2 class="d-flex justify-content-between align-items-center" @click="getStatusPlayer" >
          <!--  @click="getStatusPlayer" -->
          <b-container >
            <b-row align-h="around">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon icon="display"></b-icon>
              </b-col>

              <b-col cols="3">
                <span class="text-center"> {{sala}} </span> |
                <span class="text-center"> Tv{{tv}} </span>
              </b-col>

              <b-col cols="4">
                <span class="text-center"> <b> Emision </b> </span> |
                <span class="text-center"> {{receiveNews.channel}} </span>
              </b-col>
      
            </b-row>

          </b-container>
            

        </b-list-group-item>

          <b-collapse id="collapse-2" class="mb-3" >
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

                <!-- button for a url to change streaming -->
                <!-- <b-row align-h="center">
                  <b-col cols="8">
                      <b-form-input size="sm" class="mt-3" v-model="newStreaming" placeholder="Enter url Streaming rtsp://ip/0"></b-form-input>
                  </b-col>
                  <b-col cols="4" style="margin-left: -25px">
                      <b-button size="sm" variant="success" class="mt-3" > Cambiar Emision </b-button>
                  </b-col>
                </b-row> --> 
                <!--  button for a url to change streaming -->

                <b-row class="mt-1" align-h="center">
                  
                  <b-col cols="4">
                    <b-button size="sm" variant="danger" class="h4 mt-3" @click="doRestart(payloads.restartPlayer)"> Reiniciar Reproductor </b-button>
                  </b-col>
                  <b-col cols="4">
                    <b-button size="sm" variant="danger" class="mt-3" @click="doRestart(payloads.restartDevice)"> Reiniciar Player </b-button>
                  </b-col>
                </b-row>
                {{receiveNews}}
                </b-container>
            </b-card>
          </b-collapse>
    </b-list-group>                        
  </b-container>
</template>


<script>
const mqtt = require('async-mqtt')

export default {
  props:[
    'sala',
    'tv',
  ],

  data() {
    return {
      statusPayer: false,
      channel:'',
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
        clientId: 'webClientPlayersotano',
        username: 'emqx',
        password: 'public',
      },

      topics: {
        subscriber:{
          status:'imbanaco/principal/players/sotano/tv1/4451b1/status',
          response: 'imbanaco/principal/players/sotano/tv1/4451b1/response',
          currentStreaming: 'imbanaco/principal/players/sotano/tv1/4451b1/streaming'
        },
        publish:{
          request: 'imbanaco/principal/players/sotano/tv1/4451b1/request',
          restart: 'imbanaco/principal/players/restart',
        },
      },

      payloads:{
        status: '{ "status": "device" }',
        restartPlayer: '{ "restart": "player" }',
        restartDevice: '{ "restart": "device" }',
       },
      
      receiveNews: '',
    }
  },

  methods: {
   
   async conectar(){
       const { host, port, endpoint} = this.connection
        const connectUrl = `ws://${host}:${port}${endpoint}`
        let client = null
        try {
          client = await mqtt.connectAsync(`${connectUrl}`,this.options)
          console.log(`[ Client - Connected Successfull ]`);

        } catch (error) {
          console.log(`[ Client - Dont connected ] ${error}`)
        }
        return client
   },

   async getStatusPlayer() {
        let client = await this.conectar()
        
        await client.subscribe(this.topics.subscriber.status, { qos:2 });
        await client.subscribe(this.topics.subscriber.currentStreaming, { qos:2 });

        await client.publish(this.topics.publish.request, this.payloads.status, {qos:0});

        console.log(`suscriber success to ${this.topics.subscriber.status} `);

        client.on('message', async (topic, message) => {
        console.log(`Received message ${message} from topic ${topic}`)
        this.receiveNews = JSON.parse(message);
        
        if( topic == this.topics.subscriber.currentStreaming ){
          this.channel = this.receiveNews.channel
          
        } else if (topic == this.topics.subscriber.status ) {

          if(this.receiveNews.status === 'connected'){
             this.background = 'success'
          }else
           this.background = 'danger'
          console.log(`Cerrando Conexion al broker`);
        }
        await client.end()
      })

    },

    async doRestart(target) {

        let client = await this.conectar()

        try {
          await client.publish(this.topics.publish.request, target, {qos:0});
          console.log(`publicando`);
          
        } catch (error) {
          console.log(`Error al publicar`);
        }
    }
  },

  mounted:function () {
    this.getStatusPlayer()
  }
}


</script>