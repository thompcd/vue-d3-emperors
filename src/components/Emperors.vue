/* eslint-disable vue/max-attributes-per-line */
<template>
  <div class="emperors">
    <!-- Width = 5 emperor entries + whitespace of another emperor, Height = CanvasHeight per row + space for caption and padding under last row -->
    <svg      
      :width="`${empWidth * 5 + (empWidth - columnWidth)}`"
      :height="`${(Math.floor(emperors.length / 5) * canvasHeight + canvasHeight + textHeight + 24)}`"
      aria-labelledby="emperorGraphic"
      role="img"
    >
      <title>
        id="emperorGraphic"
        A graphic to grasp the impact of each Roman emperor
      </title>
      <!-- X = Shift right by its entry number * entry width for that row, offset origin from 0,0 to the whitespace of a single entry to pad the first column -->
      <!-- Y = Shift entry origin down by its row number. The dynamic height column is then at the top left of the entry, so shift again by the whitespace of that entry's height -->
      <g
        v-for="(emp, i) in emperors"
        :key="i"
        :transform="`translate(${i % 5 * empWidth + (empWidth - columnWidth)}, ${(Math.floor(i / 5) * canvasHeight) - emperors[i].columnSize + canvasHeight})`"
        class="emperor"
      >
        <path
          class="column column-fill column-outline"
          :transform="`scale(1,${emperors[i].columnScale})`"
          d="M22.11 46.25L111.95 46.25L111.95 149.38L22.11 149.38L22.11 46.25Z"
        />
        <path
          class="column column-top"
          :transform="`translate(1,${emperors[i].columnScale * columnPlatformHeight - columnPlatformHeight})`"
          d="M0 0L134.06 0L134.06 46.25L0 46.25L0 0Z"
        />
        <text 
          class="name"
          :x="columnWidth / 2"
          :y="`${emperors[i].columnScale * columnPlatformHeight - (columnPlatformHeight/2) + (textHeight/2)}`"
          text-anchor="middle"
        >
          {{ emp.name }}
        </text>
        <text 
          class="description"
          :x="columnWidth / 2" 
          :y="`${emperors[i].columnScale * baseColumnHeight + (textHeight)}`" 
          text-anchor="middle"
        >

          {{ (emperors[i].daysReigned / 365).toFixed(1) }} yr reign</text>
        <!-- <text class="description" :x="columnWidth / 2" :y="`${emperors[i].columnScale * columnPlatformHeight + (textHeight * 2)}`" text-anchor="middle">Age {{(emperors[i].daysLived / 365).toFixed(1)}}</text> -->
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from 'd3';
import _ from 'lodash';
import rawData from '../../data/emperors.json';

export default {
  name: 'Emperors',
  props: {
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

    const reignDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    // const lifeDurationDomain = d3.extent(rawData, function(d) {return d3.timeDay.count(new Date(d.start), new Date(d.end))});
    const columnScale = d3.scaleLinear(reignDurationDomain, [0.25, 3]);

    const baseColumnHeight = 149.5;

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
  font-weight: 500;
  }
</style>
