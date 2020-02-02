<template>
  <div class="emperors">
    <div >
        <svg :width="`${empWidth * 6}`" :height="`${(Math.floor(emperors.length / 5) * canvasHeight + canvasHeight + textHeight)}`">
            <g class="emperor" v-for="(emp, i) in emperors" :key="i" :transform="`translate(${i % 5 * empWidth + 0.5*empWidth}, ${(Math.floor(i / 5) * canvasHeight) - emperors[i].columnSize + canvasHeight})`">
                <path class="column column-fill column-outline" :transform="`scale(1,${emperors[i].columnScale})`" d="M22.11 46.25L111.95 46.25L111.95 149.38L22.11 149.38L22.11 46.25Z"></path>
                <path class="column column-top" :transform="`translate(1,${emperors[i].columnScale * columnPlatformHeight - columnPlatformHeight})`" d="M0 0L134.06 0L134.06 46.25L0 46.25L0 0Z"></path>
                <text class="name" :x="columnWidth / 2" :y="`${emperors[i].columnScale * columnPlatformHeight - (columnPlatformHeight/2) + (textHeight/2)}`" text-anchor="middle">{{emp.name}}</text>
                <text class="description" :x="columnWidth / 2" :y="`${emperors[i].columnScale * baseColumnHeight + (textHeight)}`" text-anchor="middle">{{(emperors[i].daysReigned / 365).toFixed(1)}} yr reign</text>
                <!-- <text class="description" :x="columnWidth / 2" :y="`${emperors[i].columnScale * columnPlatformHeight + (textHeight * 2)}`" text-anchor="middle">Age {{(emperors[i].daysLived / 365).toFixed(1)}}</text> -->
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
      empWidth: 247,
      empHeight: 215,
      columnWidth: 134,
      columnPlatformHeight: 46.25,
      textHeight: 18,
      baseColumnHeight: 149.5,
      canvasHeight: 400
    }
  },
  mounted(){
    const reignDuration = d3.timeDay.count(new Date(rawData.start), new Date(rawData.end));
    const lifeDuration = d3.timeDay.count(new Date(rawData.birth), new Date(rawData.death));

    const reignDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    const lifeDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    const columnScale = d3.scaleLinear(reignDurationDomain, [0.25, 3]);

    const baseColumnHeight = 149.5;
    const columnWidth = 134;
    const columnPlatformHeight = 46.25;
    const textHeight = 18;

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
        columnSize: (baseColumnHeight * columnScale(d3.timeDay.count(new Date(emp.start), new Date(emp.end)))),
        columnScale: (columnScale(d3.timeDay.count(new Date(emp.start), new Date(emp.end)))),
        id: emp.id
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.column-fill {fill:#c0cac7;}
.column-top {fill:#C9C2BF;}

text{
  font-family: 'AUGUSTUS';
  font-size: 1rem;
  text-transform: uppercase;
}
.name {
  fill:#056608;
  text-transform: uppercase;
  }
</style>
