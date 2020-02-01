<template>
  <div id="app">
    <Emperors/>
  </div>
</template>

<script>
import Emperors from './components/Emperors.vue'

export default {
  name: 'app',
  components: {
    Emperors
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
