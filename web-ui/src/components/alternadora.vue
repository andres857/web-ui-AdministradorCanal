<template>
  <b-container>
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-4 @click="dogetcurrentStreaming"  class="d-flex justify-content-between align-items-center" >
          
          <b-container >
            <b-row align-h="start">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon icon="display"></b-icon>
              </b-col>

              <b-col cols="3">
                <span class="text-center"> Administrador de Emision </span> |
              </b-col>

              <b-col cols="4">
                <span class="text-center"> <b> Emision </b> </span> |
                <span class="text-center"> {{receiveNews.currentStreaming}} </span>
              </b-col>

              <b-col cols="3">
                <span class="text-center"> Imbanaco TV </span>
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

                <b-row align-h="center">
                  <b-col cols="4" style="margin-left: -25px">
                      <b-button size="sm" variant="success" class="mt-3" @click="dochangeStreaming" > Cambiar Emision </b-button>
                  </b-col>
                </b-row>
                
                </b-container>
                
            </b-card>
          </b-collapse>
    </b-list-group>
      {{NuevoCanal}}
    <b-button variant="danger" class="mt-3" > Obtener emision actual </b-button>

                                           
  </b-container>
</template>


<script>
const mqtt = require('mqtt')

export default {
  data() {
    return {
      background:'danger',
      NuevoCanal:'',
      emision:'',
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
        clientId: 'webClientPlayerSotano',
        username: 'emqx',
        password: 'public',
      },

      topics: {
        subscriber:{
          currentStreaming:'imbanaco/principal/alternadora/rjhgejhge/currentStreaming',
        },
        publish:{
          getChannel:'imbanaco/principal/alternadora/rjhgejhge/getChannel',
          changeStreaming:'imbanaco/principal/alternadora/rjhgejhge/changeStreaming',
        },
      },

      payloads:{
        getChannel: '{ "getChannel": "true" }',
        changeStreaming:'{ "changeStreaming": "true" }',
        changeStreamingToInstitucional: '{ "changeStreaming": "Institucional" }',
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
      dogetcurrentStreaming(){
        this.client.publish(this.topics.publish.getChannel, this.payloads.getChannel, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.getChannel)
        })
      },

      dochangeStreaming(){
        this.client.publish(this.topics.publish.changeStreaming, this.payloads.changeStreaming, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.changeStreaming)
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

        this.background = 'success'
        // suscribe to topics
        
        this.client.subscribe(this.topics.subscriber.currentStreaming,  this.qos , (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        console.log('Subscribe to topics res', res)
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
      })
  }
}
</script>