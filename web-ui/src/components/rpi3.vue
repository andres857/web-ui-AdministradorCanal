<template>
  <b-container >
    <b-list-group>
        <b-list-group-item v-b-toggle.collapse-3 class="d-flex justify-content-between align-items-center">
          <b-icon icon="circle-fill" :variant="background"></b-icon>
          <span class="text-center"> Player 3</span>
          <b-icon icon="diagram2"></b-icon>
        </b-list-group-item>

          <b-collapse id="collapse-3" class="mb-3" >
            <b-card>
                <b-row>
                    <b-col lg="6">
                        
                        <span><b>Emision</b></span>
                        <span> Lili TV</span>
                        <b-button variant="danger" class="mt-2" @click="doPublish">Reiniciar</b-button>
                        
                    </b-col>
                    <b-col lg="6">
                        <b-col>
                            <b-icon icon="thermometer" font-scale="1.5"></b-icon>
                            {{receiveNews.temp}} °C
                        </b-col>
                        <b-col class="mt-2">
                            <b-icon icon="cpu" font-scale="1.5"></b-icon>
                            <span> {{receiveNews.avgload}} | {{receiveNews.currentload}}</span>
                        </b-col>
                    </b-col>
                </b-row>
                
            </b-card>
          </b-collapse>
      </b-list-group>
  </b-container>
</template>


<script>
// import mqtt from 'mqtt'
const mqtt = require('mqtt')

export default {
  data() {
    return {
      background:'danger',
      connection: {
        host: '165.227.2.248',
        port: 8083,
        endpoint: '/mqtt',
        clean: true, // Reserved session
        connectTimeout: 4000, // Time out
        reconnectPeriod: 10000, // Reconnection interval
      },
      options:{
          // Certification Information
        clientId: 'webClientPlayer3lilitv',
        username: 'emqx',
        password: 'public',
      },
      subscription: {
        topic: 'valledelili/players/b8:27:eb:0f:19:b2/status',
        qos: 0,
      },
      publishRestartPlayer: {
        topic: 'valledelili/players/b8:27:eb:0f:19:b2/config',
        qos: 0,
        payload: '{ "restart": "true" }',
      },
      receiveNews: '',
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 },
      ],
      client: {
        connected: false,
      },
      subscribeSuccess: false,
    }
  },

  methods: {
    doPublish() {
        const { topic, qos, payload } = this.publishRestartPlayer
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
        console.log('Connection succeeded!')
        // this.client.connected = true
        this.background = 'success'
        const { topic, qos } = this.subscription
        this.client.subscribe(topic, { qos }, (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        this.subscribeSuccess = true
        console.log('Subscribe to topics res', res)
        })
      })
      this.client.on('error', error => {
        console.log('Connection failed', error)
        
      })
      this.client.on('message', (topic, message) => {
        // this.receiveNews = this.receiveNews.concat(message)
        console.log(`Received message ${message} from topic ${topic}`)
        // this.receiveNews=''
        this.receiveNews = JSON.parse(message);
        console.log(this.receiveNews);
      })
  }
}
</script>