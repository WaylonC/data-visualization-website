<script>
  import { geoAlbers, geoPath, geoMercator } from "d3-geo";
  import { extent } from "d3-array";
  import { scaleLinear } from "d3-scale";
  import { onMount } from "svelte";
  import { feature } from "topojson"; //later it might be good to convert this to topojson for faster loading
  

  //map stuff
  let data = [];
  let colorScale_hisp = ()=> {};
  let colorScale_trumpdiff = ()=> {};
  const width = "960";
  const height = "500";


  //projecton stuff
  const projection = geoMercator().scale(38000).center([-98.70,29.37]).translate([487.5, 305]);
  const path = geoPath().projection(projection);



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
  }
  .border {
    stroke: #444444;
    fill: #cccccc;
  }
  .provinceShape {
      stroke: #444444;
      stroke-width: 0.5;
  }
</style>




<svg width="960" height="500">
  
  {#each data as feature}
    <path
      d={path(feature)}
      class="provinceShape"
      fill={colorScale_hisp(feature.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct)} />
  {/each}

</svg>

  


  

