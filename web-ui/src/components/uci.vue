<template>
  <b-container>
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-2 class="d-flex justify-content-between align-items-center" >
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
                <span class="text-center"> {{receiveNews.emision}} </span>
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
                <!-- button for a url to change streaming -->

                <b-row class="mt-1" align-h="center">
                  
                  <b-col cols="4">
                    <b-button size="sm" variant="danger" class="h4 mt-3"> Reiniciar Reproductor </b-button>
                  </b-col>
                  <b-col cols="4">
                    <b-button size="sm" variant="danger" class="mt-3"> Reiniciar Player </b-button>
                  </b-col>
                </b-row>
                </b-container>
            </b-card>
          </b-collapse>
    </b-list-group>                        
  </b-container>
</template>


<script>
const mqtt = require('mqtt')

export default {
  props:[
    'sala',
    'tv',
  ],

  data() {
    return {
      status:'',
      background:'danger',
      NuevoCanal:'',
      emision:'',
      newStreaming:'',

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
        clientId: 'webClientPlayerUCI',
        username: 'emqx',
        password: 'public',
      },

      topics: {
        subscriber:{
          status:'imbanaco/principal/players/sotano/tv1/4451b1/status',
        },
        publish:{
          request: 'imbanaco/principal/players/sotano/tv1/4451b1/request',
        },
      },

      payloads:{
        request: '{ "status": "player" }',
      },
      
      qos: 0,

      receiveNews: '',
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 },
      ],
    }
  },

  methods: {

   getStatusPlayer() {
        this.client.publish(this.topics.publish.request, this.payloads.request, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', this.topics.publish.request)
          
        })
    },
// ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;99999999999999999999999999999

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
        }),


        this.client.publish(this.topics.publish.request, this.payloads.request, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics |||||||||||||', this.topics.publish.request)
        })
        
      })

      this.client.on('error', error => {
        console.log('Connection failed', error)
      })

      this.client.on('message', (topic, message) => {
        // this.receiveNews = this.receiveNews.concat(message)
        console.log(`Received message ${message} from topic ${topic}`)
        this.receiveNews = JSON.parse(message);
        console.log(this.receiveNews)
        if (this.receiveNews.status === 'connected') {
             this.background = 'success'
             this.client.end()
        }else{
          this.background = 'danger'
        }
      })
  }
}
</script>