<script>
  import Home from './components/Home.vue'
  import About from './components/About.vue'
  export default {
    components: {
        'home': Home,
        'about': About
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