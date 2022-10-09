<script>
import { LayerCake, Svg, Html } from 'layercake';
import { scaleOrdinal, scaleLinear } from 'd3-scale';
import { extent } from "d3-array";
import Key from './Key-html.svelte';
import AxisX from './AxisX.svelte';
import Beeswarm from './BeeswarmForce.svelte';
import { tweened } from "svelte/motion";
import data from './for_beeswarm.js';
  
const width = 1200;
const height = 500;

const xKey = 'votes_trump_difference';
const zKey = 'hispanic_above_70';
const titleKey = 'NAME';
const rKey = 'pop_total';
  
//const r = 6;
  
const seriesNames = new Set();
const seriesColors = ['#fc0', '#000'];

export let step;






//IDK IF WE'LL NEED TO USE THIS
//-----------------------------------
//const trumpdiffExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct);
const trumpdiffExtent = extent(data, d => d[xKey]);
let trumpdiffScale = ()=> {};

trumpdiffScale = scaleLinear()
  .domain(trumpdiffExtent)
  .range([-10,10]);
  //.interpolate(interpolateRound);
//------------------------------------------


  
const dataTransformed = data
    .filter(checkNotOutlier)
    .filter(checkBelowMedian)
    .map(d => {
    
        seriesNames.add(d[zKey]);

            return {
            [titleKey]: d[xKey],
            [zKey]: d[zKey],
            [xKey]: d[xKey], //trump diff
            [rKey]: d[rKey]
            }
    })

function checkNotOutlier(temp_data) {
    if ((temp_data[xKey] < 10) && (temp_data[xKey] > -10)) {  //if you cant get the radius to change based on pop size, add this : && (temp_data[popKey] > 100)
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
  
  <div class='chart-container' style='--width:{width}; --height:{height}'>

  <LayerCake
    padding={{bottom: 15}}
    x={xKey}
    z={zKey}
    r={rKey}
    zScale={scaleOrdinal()}
    zDomain={Array.from(seriesNames)}
    zRange={seriesColors}
    data={dataTransformed}
    let:width
  >
  
    <Svg>
      <AxisX/>
      <Beeswarm
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
  
