<!-- <script context="module">
  import Viewport from 'svelte-viewport-info'
</script> -->

<script>

  export let step = 1;

  import { geoAlbers, geoPath, geoMercator } from "d3-geo";
  import { extent } from "d3-array";
  import { scaleLinear } from "d3-scale";
  import { tile } from "d3-tile";
  import { select, selectAll, zoom, zoomIdentity } from "d3";
  import { onMount } from "svelte";
  import { feature } from "topojson";
  import { tweened } from "svelte/motion";
  import { interpolate } from "d3-interpolate";
  import Feature from "../routes/Feature.svelte";
  import { select_multiple_value, xlink_attr } from "svelte/internal";


  

  //map stuff
  let data = [];
  let width = "2060";
  let height = "660";


  //projecton stuff
  const projection = geoMercator().scale(38000).center([-98.70,29.37]).translate([487.5, 305]);
  const path = geoPath().projection(projection);



  //color stuff
  let colorScale_hisp = ()=> {};
  let colorScale_trumpdiff = ()=> {};
  const opacity = tweened(0, {
    duration: 1000
  });

  //map tiling
  let pi = Math.PI,
    tau = 2 * pi;
  let my_tile = ()=> {};
  
  //experimental
  let my_zoom = () => {};
  //let zoomed = () => {};
  //let stringify = () => {};
  let k;
  let r;
  let my_svg;
  let center;
  let my_vector;
  let tiles;
  let my_transform;





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
    

    //"data" is the new geographic object to use for our features, replacing the imported json
    data = topoData.features;

    
  

    //these are functions related to the tile map beneath
    my_tile = tile()
      .size([width, height]);

    my_zoom = zoom()
      .scaleExtent([1 << 11, 1 << 14])
      .on("zoom", zoomed);

    center = projection([-98.4,29.4])
    

    //this ties the map tiling to the SVG
    select(my_svg)
      .attr("width", width)
      .attr("height", height)
      .call(my_zoom)
      .call(my_zoom.transform, zoomIdentity
          .translate(width / 2, height / 2)
          .scale(1 << 12)
          .translate(-center[0], -center[1]));

      
    function zoomed(event) {
      
      my_transform = event.transform;

      tiles = my_tile
        .scale(my_transform.k)
        .translate([my_transform.x, my_transform.y])
        ();

      select(my_vector)
        .attr("transform", my_transform)
        .style("stroke-width", 1 / my_transform.k);

      select(my_svg)
        .attr("transform", stringify(tiles.scale, tiles.translate))
        .selectAll("image")
        .data(tiles, function(d) { return d; });

      select(my_svg).exit().remove();

      select(my_svg).enter().append("image")
        .attr("xlink:href", function(d) { return "http://" + "abc"[d[1] % 3] + ".tile.openstreetmap.org/" + d[2] + "/" + d[0] + "/" + d[1] + ".png"; })
        .attr("x", function(d) { return d[0] * 256; })
        .attr("y", function(d) { return d[1] * 256; })
        .attr("width", 256)
        .attr("height", 256);
  }


  function stringify(scale, translate) {
    k = scale / 256, r = scale % 1 ? Number : Math.round;
    return "translate(" + r(translate[0] * scale) + "," + r(translate[1] * scale) + ") scale(" + k + ")";
  }


  
  
  });

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


<select bind:value={step}>
		<option value=1> 1 </option>
    <option value=2> 2 </option>
</select>


<!-- <svelte:body
  on:viewportchanged={() => {
    width=Viewport.width;
    height=Viewport.height;
  }}
/> -->

<svg {width} {height} bind:this={my_svg}>


    <!-- I think we can delete this below?-->
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
    step={step}
    bind:this={my_vector}
  />
      
  {/each}

  </svg>





  

