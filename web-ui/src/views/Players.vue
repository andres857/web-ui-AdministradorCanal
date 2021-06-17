<template>
  <div class="mt-3">
    <b-row align-h="center" class="mb-5">
        <b-col cols="4">
             <b-navbar type="light" variant="light">
              <b-nav-form>
                <b-form-input class="mr-sm-2" placeholder="Buscar"></b-form-input>
                <b-button variant="outline-success" class="my-2 my-sm-0" type="submit">Search</b-button>
              </b-nav-form>
            </b-navbar>
        </b-col>
    </b-row>


    <b-row align-h="center">
      <b-col cols="5" >
          <urgencias sala="Urgencias" tv="1"/>
      </b-col>
      
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <uci sala="UCI" tv="1"/>
      </b-col>
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <esperaadultos sala="Espera Adultos" tv="1"/>
      </b-col>
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <radioterapia sala="Radioterapia" tv="1"/>
      </b-col>
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <radioterapia2 sala="Radioterapia" tv="2"/>
      </b-col>
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <medicinanuclear sala="Medicina Nuclear" tv="1"/>
      </b-col>
    </b-row>

    <b-row  align-h="center">
      <b-col cols="5">
          <resonanciamagnetica sala="Resonancia Magnetica" tv="1"/>
      </b-col>
    </b-row>
    

  </div>
</template>

<script>

// import alternadora from '../components/alternadora.vue'
import uci from '../components/uci.vue'
import urgencias from '../components/urgencias.vue'
import esperaadultos from '../components/esperaadultos.vue'
import radioterapia from '../components/radioterapia.vue'
import radioterapia2 from '../components/radioterapia2.vue'
import medicinanuclear from '../components/medicinanuclear.vue'
import resonanciamagnetica from '../components/resonanciamagnetica.vue'








export default {
  name: 'App',
  components: {
    // alternadora,
    uci,    
    urgencias,
    esperaadultos,
    radioterapia,
    radioterapia2,
    medicinanuclear,
    resonanciamagnetica
  }
}
</script>