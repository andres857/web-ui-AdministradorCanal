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
      <b-col cols="12" md="8" lg="3">
          <rpi1/>
      </b-col>
      <b-col cols="12" md="8" lg="3">
          <rpi2/>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import rpi2 from '../components/rpi2.vue'
import rpi1 from '../components/rpi1.vue'

export default {
  name: 'App',
  components: {
    rpi1,
    rpi2,
    
  }
}
</script>