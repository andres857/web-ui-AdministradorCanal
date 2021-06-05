<template>
  <b-container>
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-3 @click="getStatusPlayer"  class="d-flex justify-content-between align-items-center" >
          
          <b-container >
            <b-row align-h="start">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon icon="display"></b-icon>
              </b-col>

              <b-col cols="3">
                <span class="text-center"> Sotano </span> |
                <span class="text-center"> Tv1 </span>
              </b-col>

              <b-col cols="6">
                <span class="text-center"> <b> Emision </b> </span> |
                <span class="text-center"> Comercial : Caracol 135 </span>
              </b-col>
      
            </b-row>

          </b-container>
            

        </b-list-group-item>

          <b-collapse id="collapse-3" class="mb-3" >
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
                  <b-col cols="8">
                      <b-form-input size="sm" class="mt-3" v-model="NuevoCanal" placeholder="Enter url Streaming rtsp://ip/0"></b-form-input>
                  </b-col>
                  <b-col cols="4" style="margin-left: -25px">
                      <b-button size="sm" variant="success" class="mt-3" @click="doPublishUrlStreaming"> Cambiar Emision </b-button>
                  </b-col>

                </b-row>
                <b-row class="mt-1" align-h="center">
                  
                  <b-col cols="5">
                    <b-button variant="danger" class="h4 mt-3" @click="doPublishpublishRestartPlayer"> Reiniciar Reproductor </b-button>
                  </b-col>
                  <b-col cols="4">
                    <b-button variant="danger" class="mt-3" @click="doPublishRestartDevice"> Reiniciar Player </b-button>
                  </b-col>

                </b-row>
                </b-container>
                
            </b-card>
          </b-collapse>
    </b-list-group>
    {{NuevoCanal}}
  </b-container>
</template>


<script>
const mqtt = require('mqtt')

export default {
  data() {
    return {
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
        clientId: 'webClientPlayerurgencias',
        username: 'emqx',
        password: 'public',
      },

      subscription: {
        topics: 'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/status',
        qos: 0,
      },
      
      publishRestartDevice: {
        topic: 'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/config',
        qos: 0,
        payload: '{ "restart": "device" }',
      },

      publishRestartPlayer: {
        topic: 'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/config',
        qos: 0,
        payload: '{ "restart": "player" }',
      },

      publishUrlStreaming: {
        topic: 'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/urlStreaming',
        qos: 0,
        payload: '{ "urlStreaming":"rtsp://192.168.5.223/InstitucionalTv" }',
      },

      publishGetStatus: {
        topic: 'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/getstatus',
        qos: 0,
        payload: '{ "getstatus": "true" }',
      },

      receiveNews: '',

      topics: [
          {obtenerestatus:'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/getstatus'},
          {publicarRestartPlayer:'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/getstatus'},
          {publicarRestartDevice:'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/getstatus'},
          {publicarUrlStreaming:'imbanaco/principal/players/urgencias/tv1/ef8226d35f8246db8ea4fc7a4292f9a4/getstatus'},
        ],

      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 },
      ],
    }
  },

  methods: {

    doPublishpublishRestartPlayer() {
        const { topic, qos, payload } = this.publishRestartPlayer
        this.client.publish(this.topics.publicarRestartPlayer, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topics', this.topics.publicarRestartPlayer)
        })
    },
    
    doPublishRestartDevice() {
        const { topic, qos, payload } = this.publish
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
        })
    },

    // doPublishpublishRestartPlayer() {
    //     const { topic, qos, payload } = this.publishRestartPlayer
    //     this.client.publish(topic, payload, qos, error => {
    //       if (error) {
    //         console.log('Publish error', error)
    //       }
    //       console.log('Publish  to topics', topic)
    //     })
    // },

    doPublishUrlStreaming() {
        const { topic, qos, payload } = this.publishUrlStreaming
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
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
        const { topics, qos } = this.subscription
        this.client.subscribe(topics, { qos }, (error, res) => {
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


