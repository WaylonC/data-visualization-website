<script>

  export let step = 0;

  import { geoAlbers, geoPath, geoMercator } from "d3-geo";
  import { extent } from "d3-array";
  import { scaleLinear , scaleThreshold} from "d3-scale";
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
  
  export let w = "800";
  export let h = "700";
  const width = w;
  const height = h;
  
  //const width = "960";
  //const height = "700";


  

  //projecton stuff
  //const projection = geoMercator().scale(38000).center([-98.5,29.50]).translate([487.5, 305]);  //DEFAULT SETTING
  const projection = geoMercator().scale(50000).center([-98.3,29.50]).translate([487.5, 305]);
  const path = geoPath().projection(projection);



  //color stuff
  
  //let colorScale_hisp = ()=> {};
  let colorScale_votes_2020 = ()=> {};
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
        
    //const votesExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_pop_hisp_pct);
    // colorScale_hisp = scaleLinear()
    //   .domain(hispExtent)
    //   .range(["#feedde", "#fd8d3c"]);
    colorScale_votes_2020 = scaleThreshold()
       //.domain([0,17,33,50,67,83,100])
       //.domain([0,25,50,75,100])
       .domain([0,17,33,50,67,83,100])
       .range(['black','#47abd8','#6EBFE2','#95D2EC','#FF4242','#E82F2F','#D01B1B','white']);
       //.domain([0,50,100])
       //.range(['black','blue','red']);

       //#D01B1B, #FF4242, #FFFFFF, #e7f9ff, #95D2EC and #47abd8


    //const trumpdiffExtent = extent(topoData.features, d => d.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference);
    //colorScale_trumpdiff = scaleLinear()
    //  .domain(trumpdiffExtent)
    //  .range(["#feedde", "#fd8d3c"]);

    colorScale_trumpdiff = scaleThreshold()
      .domain([-6,-3,0,3,6])
      .range(['#47abd8','#6EBFE2','#95D2EC','#FF4242','#E82F2F','#D01B1B']);
      //.range(['blue','red','black']);
    

  

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
      .attr("xlink:href", function(d) {return "http://stamen-tiles.a.ssl.fastly.net/toner/" + d[2] + "/" + d[0] + "/" + d[1] + ".png";})
      //.attr("xlink:href", function(d) {return "http://" + "abc"[d[1] % 3] + ".tile.openstreetmap.org/" + d[2] + "/" + d[0] + "/" + d[1] + ".png";})
      .attr("x", d => (d[0] + tiles.translate[0]) * tiles.scale )  //OOOHHH.. so the problem has something to do with how "tiles.blah" is referred...
      .attr("y", function(d) { return (d[1] + tiles.translate[1]) * tiles.scale; })
      .attr("width", tiles.scale)
      .attr("height", tiles.scale);

      console.log(tiles.translate);
      console.log("inside the map the width value is " + width);
      console.log("inside the map the height value is " + height);
    
  
  
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
  
  #test::after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 1000px;  /*100%;*/    /*WHEN TWEAKING FOR SCREEN SIZES, THIS WILL NEED TO BE LOOKED AT!!!!*/
  height: 750px;/*100%;*/
  /*background-image: radial-gradient(circle closest-side at center,
    rgba(255, 255, 255, 0) 0,
    rgba(255, 255, 255, 0) 75%,
    rgba(255, 255, 255, 1) 85%
  );*/
  /*box-shadow: inset 0 0 90px 140px rgb(255 255 255);*/
}



  .borders {
    fill: #ddd;
  }
  
</style>


<!-- <select bind:value={step}>
		<option value=1> 1 </option>
    <option value=2> 2 </option>
</select> -->



<div id="test">
<svg width={width} height={height} bind:this={my_thing}>


  
  <path
    class="borders"
    d={projection}
    style="opacity: {$opacity}" />

     <!-- d is the shape of the shape of the path -->


  {#each data as feature}

 
  <Feature
    featurePath={path(feature)}
    initialColor={colorScale_votes_2020(feature.properties.precincts_pops_votes_small_precincts_included_votes_trump_pct_2020)}
    futureColor={colorScale_trumpdiff(feature.properties.precincts_pops_votes_small_precincts_included_votes_trump_difference)}
    step={step} />


  {/each}

</svg>
</div>