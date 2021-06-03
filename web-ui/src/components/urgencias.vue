<template>
  <b-container >
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-1 @click="loadinfo" class="d-flex justify-content-between align-items-center" >
          <b-icon icon="circle-fill" :variant="background"></b-icon>
          <span class="text-center"> Consulta externa </span>
          <b-icon icon="diagram2"></b-icon>
        </b-list-group-item>

          <b-collapse id="collapse-1" class="mb-3" >
            <b-card>
                <b-row v-if="loading">

                    <b-col  lg="6">
                        <span><b>Emision</b></span>
                        <span> Imbanaco <br> </span>
                        
                        <span><strong>IPv4</strong></span>
                        <span> {{receiveNews.ip4}}</span>
                        <br>
                        <span><strong>MAC</strong></span>
                        <span> {{receiveNews.MAC}}</span> 
                                                
                    </b-col>

                    <b-col lg="6">
                        <b-col>
                            <b-icon icon="thermometer" font-scale="1.5"></b-icon>
                            {{receiveNews.main}} °C
                        </b-col>
                        <b-col class="mt-2">
                            <b-icon icon="cpu" font-scale="1.5"></b-icon>
                            <span> {{receiveNews.currentLoad}} | </span>
                        </b-col>
                    </b-col>
                </b-row>
                <b-row align-h="center" v-else>
                  <b-col cols="2">
                    <b-icon icon="arrow-clockwise" animation="spin" font-scale="2"></b-icon>
                  </b-col>
                </b-row>
                <b-row align-h="around">
                  <b-col lg="4">
                    <b-button variant="danger" class="h4 mt-3" @click="doPublishpublishRestartPlayer"> Reiniciar Reproductor </b-button>
                  </b-col>
                  <b-col lg="4">

                    <b-button variant="danger" class="mt-3" @click="doPublishRestartDevice"> Reiniciar Player </b-button>
                  </b-col>
                                      <b-button variant="danger" class="mt-3" @click="doPublishUrlStreaming"> Cambiar UrlStreaming </b-button>
                </b-row>
                
            </b-card>
          </b-collapse>
      </b-list-group>
  </b-container>
</template>


<script>
const mqtt = require('mqtt')

export default {
  data() {
    return {
      loading: false,
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
        clientId: 'webClientPlayerConsulta externa',
        username: 'emqx',
        password: 'public',
      },
      subscription: {
        topics: ['imbanaco/principal/players/Consulta externa/tv1/b82ff49997ab44e98f5992f1e5522dc2/load',
                'imbanaco/principal/players/Consulta externa/tv1/b82ff49997ab44e98f5992f1e5522dc2/network'],
        qos: 0,
      },
      
      publishRestartDevice: {
        topic: 'imbanaco/principal/players/Consulta externa/tv1/b82ff49997ab44e98f5992f1e5522dc2/config',
        qos: 0,
        payload: '{ "restart": "device" }',
      },

      publishRestartPlayer: {
        topic: 'imbanaco/principal/players/Consulta externa/tv1/b82ff49997ab44e98f5992f1e5522dc2/config',
        qos: 0,
        payload: '{ "restart": "player" }',
      },

      publishUrlStreaming: {
        topic: 'imbanaco/principal/players/Consulta externa/tv1/b82ff49997ab44e98f5992f1e5522dc2/urlStreaming',
        qos: 0,
        payload: '{ "urlStreaming":"rtsp://192.168.5.223/InstitucionalTv" }',
      },

      receiveNews: '',
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 },
      ],
    }
  },

  methods: {
    doPublishRestartDevice() {
        const { topic, qos, payload } = this.publishRestartDevice
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
        })
    },

    doPublishpublishRestartPlayer() {
        const { topic, qos, payload } = this.publishRestartPlayer
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
        })
    },

    doPublishUrlStreaming() {
        const { topic, qos, payload } = this.publishUrlStreaming
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
        })
    },

    loadinfo(){
      this.loading = false;
         
        //Waste 5 seconds
        setTimeout(() => {
           this.loading = true;
        }, 2000)
    }
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
        console.log(this.receiveNews);
        // this.receiveNews=''
      })
  }
}
</script>