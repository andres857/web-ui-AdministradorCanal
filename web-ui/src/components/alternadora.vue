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

              <b-col cols="3">
                <span class="text-center"> {{sala}} </span> 
              </b-col>

              <b-col cols="4">
                <span class="text-center"> <b> Emision </b> </span> |
                <span class="text-center"> {{receiveNews.emision}} </span>
              </b-col>

              <b-col cols="3">
                <span class="text-center"> Imbanaco TV  </span>
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
          <b-img width="250" height="150" :src="imbanaco" style="cursor: pointer" @click="doChangeChannelImbanaco"></b-img>
      </b-col>  

      <b-col cols='2' class="p-2" >
        <b-img width="150" height="150" :src="rcn" style="cursor: pointer" @click="doChangeChannelRcn"></b-img>

      </b-col>

      <b-col cols='2' class="p-2" >
        <b-img width="150" height="150" :src="colombia" style="cursor: pointer" @click="doChangeChannelColombia"></b-img>
      </b-col>

      <b-col cols='2' class="p-3"  >
        <b-img width="120" height="120" :src="caracol" style="cursor: pointer" @click="doChangeChannelCaracol"></b-img>
      </b-col>

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
      colombia: require('../assets/Colombia.svg'),
      rcn: require('../assets/rcn.svg'),
      imbanaco: require('../assets/imbanaco.svg'),
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
        clientId: 'webClientAlternadora',
        username: 'emqx',
        password: 'public',
      },

      topics: {
        subscriber:{
          currentStreaming:'imbanaco/principal/alternadora/rjhgejhge/currentStreaming',
          status:'imbanaco/principal/alternadora/rjhgejhge/status',
        },
        publish:{
          getChannel:'imbanaco/principal/alternadora/rjhgejhge/getChannel',
          getStatus:'imbanaco/principal/alternadora/rjhgejhge/getStatus',
          channel:'imbanaco/principal/alternadora/rjhgejhge/channel'
        },
      },

      payloads:{
        getChannel: '{ "getChannel": "true" }',
        getStatus:'{ "status": "get" }',
        changeStreamingToInstitucional: '{ "changeStreaming": "Institucional" }',

        changeChangeCaracol: '{ "channel": "caracol" }',
        changeChangeColombia: '{ "channel": "colombia" }',
        changeChangercn: '{ "channel": "rcn" }',
        changeChangeImbanaco: '{ "channel": "imbanaco" }',
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

    doChangeChannelCaracol(){
        this.client.publish(this.topics.publish.channel, this.payloads.changeChangeCaracol, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.channel)
        })
      },

      doChangeChannelColombia(){
        this.client.publish(this.topics.publish.channel, this.payloads.changeChangeColombia, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.channel)
        })
      },

      doChangeChannelRcn(){
        this.client.publish(this.topics.publish.channel, this.payloads.changeChangercn, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.channel)
        })
      },

      doChangeChannelImbanaco(){
        this.client.publish(this.topics.publish.channel, this.payloads.changeChangeImbanaco, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish in the topic', this.topics.publish.channel)
        })
      },

      
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


        this.client.subscribe(this.topics.subscriber.status,  this.qos , (error, res) => {
            if (error) {
                console.log('Subscribe to topics error', error)
                return
            }
        console.log('Subscribe to topics res', res)
        })

        this.client.publish(this.topics.publish.getStatus, this.payloads.getStatus, this.qos, error => {
          if (error) {
            console.log('Publish error', error)
          }
          console.log('Publish  to topics', this.topics.publish.getStatus)
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