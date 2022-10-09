<script>

export let step = 0;

import { LayerCake, Svg, Html } from 'layercake';
import { scaleOrdinal, scaleLinear, scaleBand, scalePoint, scaleQuantize, scaleSequential } from 'd3-scale';
import { extent } from "d3-array";
import Key from './Key-html.svelte';
import AxisX from './AxisX.svelte';
import Beeswarm from './BeeswarmForce.svelte';
import { tweened } from "svelte/motion";
import data from './for_beeswarm.js';
  
const width = 1200;
const height = 500;

const xKey = 'votes_trump_difference';

//const zKey = 'hispanic_above_70';
const zKey = 'pop_hisp_pct';
const titleKey = 'hispanic_above_70';
const rKey = 'pop_total';
  
//const r = 6;
  
const seriesNames = new Set();
const seriesColors = ['#fc0', '#000'];



//const hispExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference);
let colorScale_hisp = scaleLinear()
    .domain([0,100])
    .range(["#53bd1a","#b5321b"]);
  

// let seriesColors_better = [];

// for (let i = 0; i < 100; i+=5) {
//   seriesColors_better.push(colorScale_hisp(i));
// }

//const seriesColors_better = ['#ffdecc', '#ffc09c', '#ffa06b', '#ff7a33'];

const seriesColors_better = [
'#FBFAFE',
'#E7E1F8',
'#D3C7F3',
'#BEAEED',
'#A894E7',
'#937BE2',
'#7d61dc',
'#794CB2',
'#6B3887',
'#53245B',
'#2E122C'
]





console.log(seriesColors_better);
//console.log(seriesColors);




  

const dataTransformed = data
    .filter(checkNotOutlier)
    //.filter(checkBelowMedian)
    .map(d => {
    
        seriesNames.add(d[zKey]);

            return {
            [titleKey]: d[titleKey],//(Math.random() * 2 - 1),//d[xKey],
            [zKey]: d[zKey],
            [xKey]: d[xKey], //trump diff
            [rKey]: d[rKey]
            }
    })


function checkNotOutlier(temp_data) {
    if ((temp_data[xKey] < 10) && (temp_data[xKey] > -10)) {  //if you cannot get the radius to change based on pop size, add this : && (temp_data[popKey] > 100)
        return temp_data;
    }
}

function checkBelowMedian(temp_data) {
    //if (temp_data[zKey] == 'More than 70' ) {
        return temp_data;
    //}
}




  
  </script>
  
  <style>
  /*
    The wrapper div needs to have an explicit width and height in CSS.
    It can also be a flexbox child or CSS grid element.
    The point being it needs dimensions since the <LayerCake> element will
    expand to fill it.
  */
  .chart-container {
    width: calc(var(--width) * 1px);
    height: calc(var(--height) * 1px);
  }
  </style>

  <select bind:value={step}>
    <option value=0> 0 </option>
    <option value=1> 1 </option>
    <option value=2> 2 </option>
  </select>



  <div class='chart-container' style='--width:{width}; --height:{height}'>

 
  <LayerCake
    padding={{bottom: 15}}
    x={xKey}
    z={zKey}
    r={rKey}
    zScale={scaleQuantize()}
    zRange={seriesColors_better}
    data={dataTransformed}
    let:width
  >
  
  
    <Svg>
      <AxisX/>
      <Beeswarm
        step={step}
        width={width}
        strokeWidth={1}
        xStrength={0.95}
        yStrength={0.075}
        getTitle={d => d[titleKey]}
      />
    </Svg>
  
    <Html pointerEvents={false}>
      <Key shape='circle' />
    </Html>
  
  </LayerCake>

  </div>

  <!--
    BACKUP LAYERCAKE!!!

 <LayerCake
    padding={{bottom: 15}}
    x={xKey}
    z={zKey}
    r={rKey}
    zScale={scaleOrdinal()}
    zDomain={Array.from(seriesNames)}
    zRange={seriesColors_better}
    data={dataTransformed}
    let:width
  >



  -->

  <!-- zDomain={[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100]}
   -->