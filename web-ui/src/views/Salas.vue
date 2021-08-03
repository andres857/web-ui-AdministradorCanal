<template>
  <b-container>
    <b-row class="mb-5 MD_contAllTT_salas">
      <div class="TT_indentificador_salas"></div>
      <b-col class="MD_contTT_salas">
        <h2>Salas de espera</h2>
      </b-col>
    </b-row>
    <!-- Salas de espera -->
    <b-row class="MD_contAll_salas">
      <b-col>
        <sala/>
      </b-col>
      <b-col>
        <sala/>
      </b-col>
      <b-col>
        <sala/>
      </b-col>
    </b-row>
    <!-- segunda fila -->
    <b-row class="mt-5 mb-5">
      <b-col>
        <sala/>
      </b-col>
      <b-col>
        <sala/>
      </b-col>
      <b-col>
        <sala/>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import sala from '../components/sala.vue'
export default {
  components: {
    sala,
  }
}

let $body = document.querySelector("body");

$body.style.backgroundColor = "#F4F8F8";
</script>

<style  scoped>
*{
  font-family: Calibri,Candara,Segoe,Segoe UI,Optima,Arial,sans-serif; 
}

div.MD_contAllTT_salas {
  margin-top: 34px !important;
  margin-bottom: 30px !important;
  align-items: center;
}

.TT_indentificador_salas{
  background: #00B2A9;
  height: 25px;
  width: 7px;
  border-radius: 1px;
}

.MD_contTT_salas{
  padding: 0px;
}

.MD_contTT_salas h2{
  margin-left: 8px;
  font-weight: 700;
  font-size: 35px;
}
</style>