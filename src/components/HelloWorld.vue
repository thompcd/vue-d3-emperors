<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
  </div>
</template>

<script>
/* eslint-disable */
import * as d3 from 'd3';
import _ from 'lodash';
import rawData from '../../data/emperors.json';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      emperors: [],
      empWidth: 274,
      empHeight: 215
    }
  },
  mounted(){
    this.episodes = _.map(rawData, emp => {
      
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
