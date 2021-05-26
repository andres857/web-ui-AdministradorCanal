
<template>
  <div>
        <b-row>
          <b-col class="pt-2">
              <b-button variant="success" @click="createConnection">Crear Conexion</b-button>
          </b-col>

          <b-col class="pt-2">
              <b-button variant="success" @click="dosubscribe">Subscribe statusPC</b-button>
              <p>Subscribe to topic  {{subscription.topic}}</p>
          </b-col>

          <b-col>  
              <b-button variant="primary" @click="doPublishConfig">Publish Config</b-button>
              <p>Publish to topic  {{publishConfig.topic}}</p>
          </b-col>
          
      </b-row>

      <b-row>
                <b-list-group>
        <b-list-group-item v-b-toggle.collapse-1 class="d-flex  justify-content-between align-items-center">
            <b-row align-h="center">
                <b-col>
                    <b-icon icon="circle-fill" :variant="background"></b-icon>
                </b-col>
                <b-col>
                    <span class="text-center"> statusPC </span>
                </b-col>
            </b-row>
              </b-list-group-item>

              <b-collapse id="collapse-1" class="mb-3" >
              <b-card>
                  <b-row>
                      <b-col>
                          {{receiveNews.idPlayer}}<br>
                          <span><strong>Emision</strong></span>
                          <span> Imbanaco TV</span>
                          <br>
                          <span><strong>IPv4</strong></span>
                          
                          <span> 192.168.0.15</span>
                          <br>
                          <span><strong>MAC</strong></span>
                          <span> {{receiveNews.idPlayer}}</span>
                          
                      </b-col>
                      <b-col>
                          <span><strong>Temperatura </strong></span>
                          <span> {{receiveNews.temp}}</span>
                          <br>
                          <span><strong>Carga Promedio </strong></span>
                          <span>{{receiveNews.avgload}}</span>
                          <br>
                          <span><strong>Carga Actual </strong></span>
                          <span> {{receiveNews.currentload}}</span>
                      </b-col>
                  </b-row>
                  <b-button variant="danger" class="mt-2" @click="doPublishConfig">Reiniciar</b-button>
              </b-card>
              </b-collapse>
          </b-list-group>
      </b-row>
  </div>
</template>


<script>
// import mqtt from 'mqtt'
const mqtt = require('mqtt')

export default {
  data() {
    return {
      background:'danger',
      connection: {
        host: 'broker.windowschannel.us',
        port: 8084,
        endpoint: '/mqtt',
        clean: true, // Reserved session
        connectTimeout: 4000, // Time out
        reconnectPeriod: 4000, // Reconnection interval
      },
      options:{
          // Certification Information
        clientId: 'mqttjs_3be2c321',
        username: 'emqx_test_web',
        password: 'emqx_test',
      },
      subscription: {
        topic: 'imbanaco/players/0a:1b:d6:61:7e:88/status',
        qos: 0,
      },
      publishConfig: {
        topic: 'imbanaco/players/0a:1b:d6:61:7e:88/config',
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
    // Create connection
    createConnection() {
      const { host, port, endpoint} = this.connection
      const connectUrl = `wss://${host}:${port}${endpoint}`
      try {
        this.client = mqtt.connect(connectUrl,this.options)
      } catch (error) {
        console.log('mqtt.connect error', error)
      }
      this.client.on('connect', () => {
        console.log('Connection succeeded!')
        // this.client.connected = true
        this.background = 'success'
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
    },

    dosubscribe() {
        const { topic, qos } = this.subscription
        this.client.subscribe(topic, { qos }, (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        this.subscribeSuccess = true
        console.log('Subscribe to topics res', res)
        })
    },
    doPublishConfig() {
        const { topic, qos, payload } = this.publishConfig
        this.client.publish(topic, payload, qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', topic)
        })
      },
  }
}
</script>