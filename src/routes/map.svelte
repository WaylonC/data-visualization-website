<script>

  export let step = 1;

  import { geoAlbers, geoPath, geoMercator } from "d3-geo";
  import { extent } from "d3-array";
  import { scaleLinear } from "d3-scale";
  import { tile } from "d3-tile";
  import { select, selectAll } from "d3";
  import { onMount } from "svelte";
  import { feature } from "topojson";
  import { tweened } from "svelte/motion";
  import { interpolate } from "d3-interpolate";
  import Feature from "./Feature.svelte";
    import { select_multiple_value, xlink_attr } from "svelte/internal";


  

  //map stuff
  let data = [];
  const width = "960";
  const height = "700";


  //projecton stuff
  const projection = geoMercator().scale(38000).center([-98.5,29.47]).translate([487.5, 305]);
  const path = geoPath().projection(projection);



  //color stuff
  let colorScale_hisp = ()=> {};
  let colorScale_trumpdiff = ()=> {};
  const opacity = tweened(0, {
    duration: 1000
  });

  //crazy thing
  let pi = Math.PI,
    tau = 2 * pi;

  let tiles = ()=> {};

  let my_thing;



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

    
  

      
    tiles = tile()
      .size([width, height])
      .scale(projection.scale() * tau)
      .translate(projection([0, 0]))
      ();


    select(my_thing)
      .selectAll("image")
      .data(tiles)
      .enter().append("image")
      .attr("xlink:href", function(d) {return "http://" + "abc"[d[1] % 3] + ".tile.openstreetmap.org/" + d[2] + "/" + d[0] + "/" + d[1] + ".png";})
      .attr("x", d => (d[0] + tiles.translate[0]) * tiles.scale )  //OOOHHH.. so the problem has something to do with how "tiles.blah" is referred...
      .attr("y", function(d) { return (d[1] + tiles.translate[1]) * tiles.scale; })
      .attr("width", tiles.scale)
      .attr("height", tiles.scale);

      console.log(tiles.translate);
    
  
  
  });



  //MORE CRAZY STUFF


 

    //my homework, when i return to this, is to look at the functioning html page. Look at what "tiles.translate[0]" is SUPPOSED to return. 


      //.attr("x", d => (d[0] + tiles.translate[0]) * tiles.scale )  //OOOHHH.. so the problem has something to do with how "tiles.blah" is referred...
      //.attr("y", function(d) { return (d[1] + tiles.translate[1]) * tiles.scale; })
      //.attr("width",256);
      //.attr("width", tiles.scale)
      //.attr("height", tiles.scale);




//import Icon from "$lib/data/favicon.png"; //this is how you refer to static files?
</script>

<style>



  /*da regular stuff to keep */
  svg {
    width: width;
    height: height;
    background-color: "#eeeeee";
  }
  .borders {
    fill: #ddd;
  }
  
</style>


<!-- <select bind:value={step}>
		<option value=1> 1 </option>
    <option value=2> 2 </option>
</select> -->




<svg width={width} height={height} bind:this={my_thing}>


  
  <path
    class="borders"
    d={projection}
    style="opacity: {$opacity}" />

     <!-- d is the shape of the shape of the path -->


  {#each data as feature}

 
  <Feature
    featurePath={path(feature)}
    initialColor={colorScale_hisp(feature.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct)}
    futureColor={colorScale_trumpdiff(feature.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference)}
    step={step} />


  {/each}

</svg>