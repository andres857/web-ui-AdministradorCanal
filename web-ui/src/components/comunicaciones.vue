<template>
  <b-container>
    <b-row>
      <b-col>
        <b-list-group>
          <b-list-group-item>

            <b-row align-h="center">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon variant="outline-success" icon="display"></b-icon>
              </b-col>
              <b-col cols= "3" >
                 {{sala}} | Tv{{tv}}
              </b-col>
              
              <b-col cols= "3">
                <b>Emision:</b> {{receiveNews.channel}}
              </b-col>
              <b-col cols="4">
                <b-row align-h="center">
                  <b-col cols="2">
                    <div>
                      <b-button v-b-toggle="'collapse-comunicaciones'" variant="success">+</b-button>
                    </div>
                  </b-col>
                  <b-col>
                    <div>
                      <b-icon style="margin-right: 1px; fill: #00B2A9;
                      margin-right: 1px; " font-scale="2" icon="card-checklist"  @click="showModal"></b-icon>
                        <b-button v-b-tooltip.hover title="Reiniciar Reproductor" size="sm" variant="danger"  @click="doRestart(payloads.restartDevice)">  <b-icon icon="cast"></b-icon>  </b-button>    
                        <b-modal ref="my-modal" hide-footer v-b-modal.modal-sm title="Reproductor Multimedia">
                          <div>
                            <p>
                              <b>Id Reproductor</b>: 050345245
                              <br>
                              <b>Estado</b>
                            </p>

                            <div style="margin-left:10px; margin-top:-15px">
                                {{receiveNews.main}}°C <b-icon icon="thermometer" font-scale="1.5"></b-icon> <br>    
                                <span>{{receiveNews.currentLoad}} % </span>
                                <b-icon icon="cpu" font-scale="1.5"></b-icon><br>
                            </div>
                                
                            <p>
                              <b>Canal</b>: {{receiveNews.channel}} <br>
                              <b>Ubicacion</b>: {{sala}} | Tv{{tv}}<br> 
                              <b>Network:</b> <br>
                              <b style="margin-left:10px">IP</b>: {{receiveNews.ip4}}<br>
                              <b style="margin-left:10px">MAC</b>: {{receiveNews.MAC}}<br>
                            </p>
                          </div>
                          <div>
                            <b-button class="mt-3" variant="outline-danger"  @click="hideModal">Cerrar</b-button>
                          </div>
                        </b-modal>
                    </div>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
            <div>
              <!-- Element to collapse -->
              <b-collapse id="collapse-comunicaciones">
              <!-- button for a url to change streaming -->
                  <b-row align-h="center">
                    <b-col cols="8">
                        <b-form-input size="sm" class="mt-3" v-model="newStreaming" placeholder="Enter url Streaming rtsp://ip/0, o URL de youtube"></b-form-input>
                    </b-col>
                    <b-col cols="4" style="margin-left: -25px">
                        <b-button size="sm" variant="success" class="mt-3" @click="sendNewStreaming(newStreaming)"> Enviar Emision </b-button>
                    </b-col>
                    {{newStreaming}}
                  </b-row>
              <!-- button for a url to change streaming -->
              </b-collapse>
            </div>

          </b-list-group-item>
        </b-list-group>
      </b-col>
      
    </b-row>    
    
  </b-container>
</template>


<script>

const mqtt = require('async-mqtt')
let $body = document.querySelector("body");
$body.style.backgroundColor = "#F4F8F8";

export default {
  
  props:[
    'sala',
    'tv',
  ],

  data() {
    return {
      newStreaming:'',
      dismissSecs: 5,
      dismissCountDown: 0,
      client: '',
      channel:'',
      background:'danger',
      receiveNews: '',

      connection: {
        host: 'broker.windowschannel.com',
        port: 8084,
        endpoint: '/mqtt',
        clean: true, // Reserved session
        connectTimeout: 4000, // Time out
        reconnectPeriod: 10000, // Reconnection interval
      },
      options:{
          // Certification Information
        clientId: 'webClientPlayer',
        username: 'webclient',
        password: 'wchannel2020',
      },

      topics: {
        subscriber:{
          status:'windowschannel/player/status/15a6ec',
          response: 'windowschannel/player/response/15a6ec',
          currentStreaming: 'windowschannel/player/currentstreaming/15a6ec'
        },
        publish:{
          request: 'windowschannel/player/request/15a6ec',
          newStreaming: 'windowschannel/player/streaming/15a6ec',
        },
      },
      payloads:{
        status: '{ "status": "device" }',
        volume: '{ "volume": "0.8" }',//Cambiar el valor de volumen desde la vista
        restartDevice: '{ "restart": "device" }',
       },
    }
  },

  methods: {
  showModal() {
    this.$refs['my-modal'].show()
  },
  hideModal() {
    this.$refs['my-modal'].hide()
  },
  countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown
  },
  showAlert() {
    this.dismissCountDown = this.dismissSecs
  },
      
    async conectar(){
      // if (this.client) return this.client     
        const { host, port, endpoint} = this.connection
        const connectUrl = `wss://${host}:${port}${endpoint}`
        let client = null
          try {
            client = await mqtt.connectAsync(`${connectUrl}`,this.options)
            console.log(`[ Client - Connected Successfull to Broker ]`);
          } catch (error) {
            console.log(`[ Client - Error to connected to Broker : ${error} ]`)
          }
        // this.client = client
      return client
    },

    buildTopicNewStreaming(urlStreaming){
      return JSON.stringify({
        titleStreaming: 'name',
        urlStreaming: urlStreaming
      })
    },

    async getStatusPlayer() {
        let client = await this.conectar()
        
        await client.subscribe(this.topics.subscriber.status, { qos:2 });
        await client.subscribe(this.topics.subscriber.currentStreaming, { qos:2 });
        await client.subscribe(this.topics.subscriber.response, { qos:2 });

        await client.publish(this.topics.publish.request, this.payloads.status, {qos:0});

        console.log(`suscriber success to ${this.topics.subscriber.status} `);
        console.log(`suscriber success to ${this.topics.subscriber.currentStreaming} `);
        console.log(`suscriber success to ${this.topics.subscriber.response} `);


        client.on('message', async (topic, message) => {
        console.log(`Received message ${message} from topic ${topic}`)
        this.receiveNews = { ...this.receiveNews , ...JSON.parse(message)};
        if((this.receiveNews.currentLoad || this.receiveNews.main) != null){
          this.background = 'success'
        }else if( topic == this.topics.subscriber.currentStreaming ){
          this.channel = this.receiveNews.channel
          console.log(`Canal recibido del player ${this.channel}`)
          }
      })

    },
  
    async sendNewStreaming(urlStreaming){
      let client = await this.conectar()
      try {
            let streaming = this.buildTopicNewStreaming(urlStreaming)
            console.log(`------------------ publicando nuevo streaming ----------------, ${streaming}`);
            await client.publish(this.topics.publish.newStreaming, streaming, {qos:0});
            console.log(`en el topic ${this.topics.publish.newStreaming}`);
            this.showAlert()
            
          } catch (error) {
            console.log(`Error al publicar`);
          }
    },

    async doRestart(target) {

        let client = await this.conectar()

        try {
          await client.publish(this.topics.publish.request, target, {qos:0});
          
          console.log(`publicando`);
          this.showAlert()
          
        } catch (error) {
          console.log(`Error al publicar`);
        }
    },
  },



  mounted:function () {
    this.getStatusPlayer()
  }
}


</script>

<style scoped>
*{
  font-family: Calibri,Candara,Segoe,Segoe UI,Optima,Arial,sans-serif; 
}

.MD_cont_adminPlayer svg {
  margin-right: 6px;
  fill: #00B2A9;
}
</style>