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
    

  export let w = 700;
  export let h = 750;
  const height = 700;
  

  
  
  const xKey = 'votes_trump_difference';
  const zKey = 'pop_hisp_pct';
  const titleKey = 'hispanic_above_70';
  const rKey = 'pop_total';
    

    
  const seriesNames = new Set();
  const seriesColors = ['#fc0', '#000'];
  
  
  

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
  
  
    
  
  const dataTransformed = data
      .filter(checkNotOutlier)
      .map(d => {
      
          seriesNames.add(d[zKey]);
  
              return {
              [titleKey]: d[titleKey],
              [zKey]: d[zKey],
              [xKey]: d[xKey], //trump diff
              [rKey]: d[rKey]
              }

      })
  
  
  function checkNotOutlier(temp_data) {
      if ((temp_data[xKey] <= 9) && (temp_data[xKey] >= -9)) {  //if you cannot get the radius to change based on pop size, add this : && (temp_data[popKey] > 100)
          return temp_data;
      }
  }

  console.log("inside the beeswarm the width value is " + w);
  console.log("inside the beeswarm the height value is " + h); 

      
    </script>
    
    <style>
    /*
      The wrapper div needs to have an explicit width and height in CSS.
      It can also be a flexbox child or CSS grid element.
      The point being it needs dimensions since the <LayerCake> element will
      expand to fill it.
    */
    .chart-container {
      width: calc(var(--w) * 1px);
      height: calc(var(--height) * 1px);
    }
    </style>
  
    
  
  
  
    <div class='chart-container' style='--width:{w}; --height:{h}'>
    <!-- <div class='chart-container' width={width} height={height} >
   -->
   
    <LayerCake
      padding={{bottom: 15}}
      x={xKey}
      z={zKey}
      r={rKey}
      zScale={scaleQuantize()}
      zRange={seriesColors_better}
      data={dataTransformed}
       
    >
    
    
      <Svg>
        {#if step > 0 }
          <AxisX/>
        {/if}
        
        {#if w <= "800"}
          <Beeswarm
            step={step}
            width={w}
            strokeWidth={1}
            xStrength={0.95}
            yStrength={0.075}
            getTitle={d => d[titleKey]}
            rScale = 0.3
          />
        {:else}
          <Beeswarm
              step={step}
              width={w}
              strokeWidth={1}
              xStrength={0.95}
              yStrength={0.075}
              getTitle={d => d[titleKey]}
            />
        
        {/if}
      </Svg>
    
      <!-- THIS IS WHERE YOU WOULD PUT THEY KEY-->
      <!-- <Html pointerEvents={false}>
        <Key shape='circle'/>
      </Html> -->
    
    </LayerCake>
  
    </div>


