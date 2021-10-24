<template>
  <b-container>

    <b-row class="mb-5 mt-5 MD_contAllTT_players">
      <div class="TT_indentificador_players"></div>
      <b-col cols="8" class="MD_contTT_players">
        <h2>Reproductores de video</h2>
      </b-col>
    </b-row>

    <!-- <b-row  align-h="center">
      <b-col cols="12">
          <uci sala="pruebas" tv="1"/>
      </b-col>
    </b-row> -->

    <b-row  align-h="center">
      <b-col cols="12">
          <resonanciamagnetica sala="Resonancia Magnetica1" tv="1"/>
      </b-col>
    </b-row>

  </b-container>
</template>

<script>

// import uci from '../components/uci.vue'
import resonanciamagnetica from '../components/resonanciamagnetica.vue'

export default {
  name: 'App',
  components: {
    resonanciamagnetica,
    // uci,        
  }
}
</script>

<style scoped>
*{
  font-family: Calibri,Candara,Segoe,Segoe UI,Optima,Arial,sans-serif; 
}
  
div.MD_contAllTT_players {
  margin-top: 34px !important;
  margin-bottom: 30px !important;
  align-items: center;
}

.TT_indentificador_players{
  background: #00B2A9;
  height: 25px;
  width: 7px;
  border-radius: 1px;
}

.MD_contTT_players h2 {
  margin-left: 8px;
  font-weight: 700;
  font-size: 35px;
}
</style>

