<template>
  <div class="emperors">
    <h1>{{ msg }}</h1>
    <div v-for="(emp, i) in emperors" :key="i">
        <h2>{{emperors[i].name}}</h2>
        <p>{{emperors[i].dateOfBirth}}</p>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import * as d3 from 'd3';
import _ from 'lodash';
import rawData from '../../data/emperors.json';

export default {
  name: 'Emperors',
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
    this.emperors = _.map(rawData, emp => {
      return {
        name: (emp.name),
        dateOfBirth: new Date(emp.birth),
        dateOfDeath: new Date(emp.death),
        dateOfReignStart: new Date(emp.start),
        dateOfReignEnd: new Date(emp.end),
        causeOfDeath: emp.cause,
        killer: emp.killer,
        dynasty: emp.dynasty
      }
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
