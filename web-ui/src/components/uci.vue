<template>
  <b-container>
        {{receiveNews}}<hr>
        {{receiveNews.emision}}

    <b-row align-h="center">
      <b-col cols="4">
        <b-alert 
          :show="dismissCountDown"
          dismissible
          variant="success"
          @dismissed="dismissCountDown=0"
          @dismiss-count-down="countDownChanged"
        >
        <p > <b-icon variant="outline-success" icon="check-circle"></b-icon> Reinicio Exitoso! </p>
        </b-alert>
      </b-col>
    </b-row>

    <b-row>
      <b-col>
        <b-list-group>
          <b-list-group-item>
            <b-row align-h="center">

              <b-col cols="2">
                <b-icon style="margin-right: 15px" font-scale="1" icon="circle-fill" :variant="background"></b-icon>
                <b-icon variant="outline-success" icon="display"></b-icon>
              </b-col>
              <b-col cols= "4" >
                 {{sala}} | Tv{{tv}}
              </b-col>
              
              <b-col cols= "4">
                <b>Emision:</b> {{receiveNews.emision}}
              </b-col>
              <b-col cols="2">
                <div>
                  <b-icon style="margin-right: 15px; fill: #00B2A9;
                   margin-right: 6px; " font-scale="2" icon="card-checklist"  @click="showModal"></b-icon>
                   
                  <b-button v-b-tooltip.hover title="Reiniciar Aplicativo" class="mr-2" size="sm" variant="danger" @click="doRestart(payloads.restartPlayer)" >  <b-icon icon="collection-play"></b-icon> </b-button>
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
                          <b>Canal</b>: Imbanaco TV <br>
                          <b>Ubicacion</b>: {{sala}} | Tv{{tv}}<br> 
                          <b>Network:</b> <br>
                          <b style="margin-left:10px">IP</b>: {{receiveNews.ip4}}<br>
                          <b style="margin-left:10px">MAC</b>: {{receiveNews.MAC}}<br>
                          <b>Visto ultima vez</b>: {{receiveNews.lastseen}}
                        </p>
                      </div>
                      <div>
                        <b-button class="mt-3" variant="outline-danger"  @click="hideModal">Cerrar</b-button>
                      </div>
                      
                    </b-modal>
                </div>
              </b-col>
            </b-row>
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
      dismissSecs: 5,
      dismissCountDown: 0,
      client: '',
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
          status:'imbanaco/principal/players/pruebas/tv1/4451b1/status',
          response: 'imbanaco/principal/players/pruebas/tv1/4451b1/response',
          currentStreaming: 'imbanaco/principal/players/pruebas/tv1/4451b1/streaming'
        },
        publish:{
          request: 'imbanaco/principal/players/pruebas/tv1/4451b1/request',
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
          const connectUrl = `ws://${host}:${port}${endpoint}`
          let client = null
            try {
              client = await mqtt.connectAsync(`${connectUrl}`,this.options)
              console.log(`[ Client - Connected Successfull ]`);

            } catch (error) {
              console.log(`[ Client - Dont connected ] ${error}`)
            }
          // this.client = client
        return client
   },

   async getStatusPlayer() {
        let client = await this.conectar()
        
        await client.subscribe(this.topics.subscriber.status, { qos:2 });
        await client.subscribe(this.topics.subscriber.currentStreaming, { qos:2 });
        // await client.subscribe(this.topics.subscriber.response, { qos:2 });

        await client.publish(this.topics.publish.request, this.payloads.status, {qos:0});

        console.log(`suscriber success to ${this.topics.subscriber.status} `);

        client.on('message', async (topic, message) => {
        console.log(`Received message ${message} from topic ${topic}`)
        this.receiveNews = { ...this.receiveNews , ...JSON.parse(message)};
        
        if( topic == this.topics.subscriber.currentStreaming ){
          this.channel = this.receiveNews.channel
          
        } else if (topic == this.topics.subscriber.status ) {

          if(this.receiveNews.status === 'connected'){
             this.background = 'success'
          }else
           this.background = 'danger'
          await client.end()
          console.log(`Cerrando Conexion al broker`);
        }
      })

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
    }
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