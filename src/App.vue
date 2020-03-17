<template>
  <div class="bgform">
       <Header />    
       <Home />    
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import Home   from './components/home/Home.vue'

  export default {

    name: 'App',  
    components: {
      Header,
      Home
    },
  
  }
</script>

<style>
  @import 'https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css';

  .bgform {
    background-image: url('./assets/bgform1.jpg');
    background-size: cover;
    height: 100vh;}

</style>
