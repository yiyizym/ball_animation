<template>
  <div id="app">
      <Playground/>
  </div>
</template>

<script>
import Playground from './components/Playground.vue'

export default {
  name: 'App',
  components: {
    Playground
  }
}
</script>

<style>
#app {
  height: 100%;
  width: 100%;
}
</style>
