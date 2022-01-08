
<template>
  <div id="app">
    <timer></timer>
    
  </div>
</template>

 <script>
    import timer from './components/timer.vue'

    export default {
     name: 'app',
     components:{
        timer
     }
    }
   </script>

<style>
  @import './assets/style/styles.css';
</style>