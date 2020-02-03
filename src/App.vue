<template>
  <div id="app">
    <div class="flex">
      <h1>REIGNING<br>IN<br>ROMAN<br>EMPERORS</h1>
      <h4 class="subtext">
        A graphic to grasp the impact of each emperor
      </h4>
    </div>
    <Emperors />
  </div>
</template>

<script>
import Emperors from './components/Emperors.vue'

export default {
  // eslint-disable-next-line vue/name-property-casing
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
  font-family: 'AUGUSTUS';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: hsla(34, 78%, 91%,.55);
}
h1{
  font-family: 'AUGUSTUS';
  text-align: center;
  text-transform: uppercase;
  font-size: 3.5rem;
  line-height: 4rem;
}
h4{
  font-family: 'Times New Roman', Times, serif;
  text-align: left;
  font-size: 2rem;
  font-style: italic;
}
.flex{
  display: flex;
  justify-content: center;
  align-items: center;
}
.flex > *{
  padding: 1rem;
}
.subtext{
  width: 200px;
  color: gray;
}
</style>
