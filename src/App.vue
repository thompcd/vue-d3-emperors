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
@font-face{
  font-family: 'AUGUSTUS';
  src: url('~@/assets/fonts/AUGUSTUS.TTF');
  font-style: normal;
}
#app {
  font-family: 'AUGUSTUS', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
