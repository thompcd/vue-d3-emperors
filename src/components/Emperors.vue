<template>
  <div class="emperors">
    <h1>{{ msg }}</h1>
    <div >
        <!-- <h2>{{emperors[i].name}}</h2>
        <p>Born {{emperors[i].dateOfBirth}}</p>
        <p>Reigned for {{emperors[i].daysReigned}} days</p>
        <p>Lived for {{emperors[i].daysLived}} days</p> -->
        <svg width="10000" height="10000">
            <g class="emperor" v-for="(emp, i) in emperors" :key="i" :transform="`translate(${i * empWidth}, 1)`">
                <path :transform="`scale(1,${emperors[i].columnSize})`" class="column column-fill column-outline" d="M22.11 46.25L111.95 46.25L111.95 149.38L22.11 149.38L22.11 46.25Z" id="l2a2C9Xe3A"></path>
                <path :transform="`translate(1,${emperors[i].columnSize * {baseColumnHeight}})`" d="M0 0L134.06 0L134.06 46.25L0 46.25L0 0Z" id="caP2qghvLs"></path>
            </g>
        </svg>
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
    const reignDuration = d3.timeDay.count(new Date(rawData.start), new Date(rawData.end));
    const lifeDuration = d3.timeDay.count(new Date(rawData.birth), new Date(rawData.death));
    const baseColumnHeight = "149px";

    const reignDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    const lifeDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    // const twitterFollowingDomain = d3.extent(rawData, ({ guest_twitter_following }) => guest_twitter_following);
    // const sharesDomain = d3.extent(rawData, ({ shares }) => shares);
    const columnScale = d3.scaleLinear(reignDurationDomain, [0.25, 3]);
    // const wingScale = d3.scaleLinear(twitterFollowingDomain, [0.25, 1]);
    // const haloScale = d3.scaleLinear(sharesDomain, [0.25, 1]);

    this.emperors = _.map(rawData, emp => {
      return {
        name: (emp.name),
        dateOfBirth: new Date(emp.birth),
        dateOfDeath: new Date(emp.death),
        dateOfReignStart: new Date(emp.start),
        dateOfReignEnd: new Date(emp.end),
        causeOfDeath: emp.cause,
        killer: emp.killer,
        dynasty: emp.dynasty,
        daysLived: d3.timeDay.count(new Date(emp.birth), new Date(emp.death)),
        daysReigned: d3.timeDay.count(new Date(emp.start), new Date(emp.end)),
        columnSize: columnScale(d3.timeDay.count(new Date(emp.birth), new Date(emp.death)))
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.column-fill {fill:#D6A9B2;}
</style>
