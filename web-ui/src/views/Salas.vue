<template>
  <b-container>
    <b-row class="mb-5">
      <b-col>
        <h2>Salas de espera</h2>
      </b-col>
    </b-row>
    <!-- Salas de espera -->
    <b-row>
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
</script>