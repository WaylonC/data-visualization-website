<!DOCTYPE html>
<meta charset="utf-8">
<style>

#streets {
  fill: none;
  stroke: black;
  stroke-width: 1.5px;
  stroke-linejoin: round;
}

</style>
<svg width="960" height="960"></svg>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/d3-tile.v0.0.min.js"></script>
<script src="https://d3js.org/topojson.v1.min.js"></script>
<script>

var pi = Math.PI,
    tau = 2 * pi;


var width = 960;
var height = 500;

var projection = d3.geoMercator()
    .scale((1 << 18) / tau)
    .translate([width / 2, height / 2])
    .center([-83.15, 42.43]);

var path = d3.geoPath()
    .projection(projection);

d3.json("https://raw.githubusercontent.com/WaylonC/filedump/main/detroit.json", function(error, detroit) {
  if (error) throw error;

var tiles = d3.tile()
    .size([width, height])
    .scale(projection.scale() * tau)
    .translate(projection([0, 0]))
    ();


console.log(tiles.scale);
  
    d3.select("svg").
    
    selectAll("image")
      .data(tiles)
    .enter().append("image")
      .attr("xlink:href", function(d) {console.log(d[0]+" hm "+tiles.translate[0]) ; return "http://" + "abc"[d[1] % 3] + ".tile.openstreetmap.org/" + d[2] + "/" + d[0] + "/" + d[1] + ".png"; })
      .attr("x", function(d) { return (d[0] + tiles.translate[0]) * tiles.scale; })
      .attr("y", function(d) {console.log(tiles.translate); return (d[1] + tiles.translate[1]) * tiles.scale; });
      //.attr("width", tiles.scale)
      //.attr("height", tiles.scale);


      console.log(function(d) {return tiles.translate[0];});


    d3.select("svg")
    .append("path")
      .attr("id", "streets")
      .datum(topojson.mesh(detroit, detroit.objects.detroit))
      .attr("d", path);
});

</script>