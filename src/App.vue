<script>
  import Home from './components/Home.vue'
  import Navbar from './components/layouts/Navbar.vue'
  export default {
    components: {
        'home': Home,
        'navbar': Navbar
    }
  }
</script>

<template>
  <home/>
</template>

<style>
  #app {
    height: 100%;
  }
</style>