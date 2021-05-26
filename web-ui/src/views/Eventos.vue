<template>

  <b-container class="mt-5">
    <b-row>
        <b-col>
            <h2>Crea un Evento</h2>
        </b-col>
    </b-row>
    <b-row class="mt-5">
        <b-col md="auto">
            <b-calendar v-model="value" @context="onContext" locale="en-US"></b-calendar>
        </b-col>
        <b-col>
            <b-time v-model="value" locale="en" @context="onContext"></b-time>
        </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
    data() {
      return {
        value: '',
        context: null
      }
    },
    methods: {
      onContext(ctx) {
        this.context = ctx
      }
    }
  }
</script>