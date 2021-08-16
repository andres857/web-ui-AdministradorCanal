<template>
  <div id="app">
   <b-navbar toggleable="lg" type="dark" variant="dark">
     <b-container>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
          <b-navbar-brand href="/canal">
            <b-img :src='imbanacoLogo' style="cursor: pointer" ></b-img>
          </b-navbar-brand>
        <b-collapse id="nav-collapse" is-nav>

          <b-navbar-nav  style="width: 100%" align="right">
            <b-nav-item :to="{name: 'Players'}" >Players</b-nav-item>
            <!-- <b-nav-item :to="{name: 'Salas'}">Salas</b-nav-item> -->
            <b-nav-item :to="{name: 'Canal'}">Canal</b-nav-item>
            
          </b-navbar-nav> 
        </b-collapse> 
     </b-container>
   </b-navbar>
      
  <router-view/>
  </div>
</template>

<script>
  export default{
    data() {
    return{
      imbanacoLogo: require('./assets/imbanacoOff.png'),
    }
  }
  }
</script>


<style scoped>
  .navbar.navbar-dark.bg-dark{
    background-color:#00B2A9!important;
    /* E03E52 rojo */
 }
</style>
