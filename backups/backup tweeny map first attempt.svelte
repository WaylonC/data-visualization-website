<script>

  export let step = 1;

  import { geoAlbers, geoPath, geoMercator } from "d3-geo";
  import { extent } from "d3-array";
  import { scaleLinear } from "d3-scale";
  import { onMount } from "svelte";
  import { feature } from "topojson";
  import { tweened } from "svelte/motion";
  import { interpolate } from "d3-interpolate";
  import Feature from "./Feature.svelte";


  

  //map stuff
  let data = [];
  const width = "960";
  const height = "500";


  //projecton stuff
  const projection = geoMercator().scale(38000).center([-98.70,29.37]).translate([487.5, 305]);
  const path = geoPath().projection(projection);



  //color stuff
  let colorScale_hisp = ()=> {};
  let colorScale_trumpdiff = ()=> {};
  const opacity = tweened(0, {
    duration: 1000
  });

  //crazy thing
  let test_step=1;



  onMount(async function() {
    const response = await fetch(
      "https://raw.githubusercontent.com/WaylonC/filedump/main/precincts_pops_votes_all_precincts_topojson_file_no_zeroes_redundant.json"
    );
    
    const json = await response.json();
    
    const topoData = feature(json, json.objects.precincts_pops_votes_all_precincts_topojson_file_no_zeroes_redundant); 
        
    const hispExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct);
    colorScale_hisp = scaleLinear()
      .domain(hispExtent)
      .range(["#feedde", "#fd8d3c"]);


    const trumpdiffExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference);
    colorScale_trumpdiff = scaleLinear()
      .domain(trumpdiffExtent)
      .range(["#feedde", "#fd8d3c"]);
    
  

    data = topoData.features;

  
  
  });



//import Icon from "$lib/data/favicon.png"; //this is how you refer to static files?
</script>

<style>



  /*da regular stuff to keep */
  svg {
    width: 960px;
    height: 500px;
    background-color: "#eeeeee";
  }
  .borders {
    fill: #ddd;
  }
  
</style>


<select bind:value={test_step}>
		<option value=1> 1 </option>
    <option value=2> 2 </option>
</select>





<svg width="960" height="500">
  
  <path
    class="borders"
    d={projection}
    style="opacity: {$opacity}" />

  {#each data as feature}
  <Feature
    featurePath={path(feature)}
    initialColor={colorScale_hisp(feature.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct)}
    futureColor={colorScale_trumpdiff(feature.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference)}
    step={step} />
  {/each}

</svg>



  

