<!--
	@component
	Generates an SVG Beeswarm chart using a [d3-force simulation](https://github.com/d3/d3-force).
 -->
 <script>


	export let step = 0;
	

	import { getContext } from 'svelte';
	import { forceSimulation, forceX, forceY, forceCollide } from 'd3-force';
    import { loop_guard } from 'svelte/internal';
    import MyCircle from './my_circle.svelte';

	const { data, xGet, height, zGet, rGet } = getContext('LayerCake');



	//in this phase, we need to reload the simulation....

	//let nodes = $data.map((d) => ({ ...d }));
	let nodes = $data
		.filter(d => d.hispanic_above_70 == 'More than 70')  //START OUT WITH BELOW-70'S GONE
		.map((d) => ({ ...d }));

	$: {
		if (step <= 1) {
			nodes = nodes.filter(d=> d.hispanic_above_70 == 'More than 70');
		}
		if (step == 2) {
			nodes = nodes.concat($data.filter(d => d.hispanic_above_70 == 'Below 70'));
			console.log("we are now adding in all precincts");
		}

	}


	export let width;


	/** @type {Number} [strokeWidth=1] – The circle's stroke width in pixels. */
	export let strokeWidth = 1;

	/** @type {String} [stroke='#fff'] – The circle's stroke color. */
	export let stroke = '#fff';

	/** @type {Number} [xStrength=0.95] – The value passed into the `.strength` method on `forceX`. See [the documentation](https://github.com/d3/d3-force#x_strength). */
	export let xStrength = 0.95;

	/** @type {Number} [yStrength=0.075] – The value passed into the `.strength` method on `forceY`. See [the documentation](https://github.com/d3/d3-force#y_strength). */
	export let yStrength = 0.075;

	/*SCALE DOWN THE RADIUS*/
	export let rScale = 0.50;

	/** @type {Function} [getTitle] — An accessor function to get the field on the data element to display as a hover label using a `<title>` tag. */
	export let getTitle = undefined;

	


	$: simulation = forceSimulation(nodes)
		////so getX and getY here refer to the dot's actual position on the chart, not their underlying values
		 .force('x', forceX().x(d => {
			return step != 0 ? $xGet(d) : Math.random() * (((width/2)+(width/15))-((width/2)-(width/15))) + ((width/2)-(width/15)); //IF STEP IS 0, DISTRIBUTE IN CENTER, IF NOT, DISTRIBUTE NORMALLY
		 	}).strength(xStrength))
		.force('y', forceY().y($height / 2).strength(yStrength))
		.force('collide',forceCollide().radius(d => width < 400 ? ($rGet(d)*rScale)/1.25 : $rGet(d)*rScale)) //COLLISSION TAKING RADIUS INTO ACCOUNT. if svg width below 400, scale down
		.stop();
		
		
		//width < 400 ? r / 1.25 : r	


	$: {
		for ( let i = 0,
			n = 120;
			i < n;
			++i ) {
			simulation.tick();
		}
	}



</script>

<g class='bee-group'>
	{#each simulation.nodes() as node}

    
	<MyCircle
		c_fill={$zGet(node)}
		c_stroke={stroke}
		c_strokeWidth={strokeWidth}
		c_cx={node.x}
		c_cy={node.y}
		c_r={width < 400 ? ($rGet(node)*rScale) / 1.25 : $rGet(node)*rScale}
		step={step}
		c_hisp={node.hispanic_above_70}
	>


	</MyCircle>

	{/each}
</g>

