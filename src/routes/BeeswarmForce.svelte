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
		// if (step == 1) { //SCROLL TO NEXT STEP... ACTUALLY THIS IS TAKEN CARE OF IN THE FORCE SIMULATION
		// 	//nodes = nodes.filter(d => d.hispanic_above_70 == 'More than 70');
		// 	//nodes.forEach((d)=>{console.log(d);});

		// 	//nodes.forEach((d)=>{d = d[titleKey];});
		// 	//(d)=>{d[xKey] = d[titleKey];}
		// }
		if (step == 1) {
			nodes = nodes.filter(d=> d.hispanic_above_70 == 'More than 70');
		}
		if (step == 2) {
			nodes = nodes.concat($data.filter(d => d.hispanic_above_70 == 'Below 70'));
		}

	}

	//console.log("force.svelte is firing");
	//nodes.forEach((d)=>{console.log(d);});

	//console.log(nodes);
	//nodes = nodes(d => d.filter(d.hispanic_above_70 == 'More than 70'));

	//.filter(data.hispanic_above_70 == 'More than 70').

	/* @type {Number} [r=4] – The circle radius size in pixels. */
	//export let r = 4;
	//let r = 4;

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
		//.force('x', forceX().x(d => $xGet(d)).strength(xStrength))
		 .force('x', forceX().x(d => {
			//return $xGet(d);
			return step != 0 ? $xGet(d) : Math.random() * (((width/2)+(width/15))-((width/2)-(width/15))) + ((width/2)-(width/15)); //IF STEP IS 0, DISTRIBUTE IN CENTER, IF NOT, DISTRIBUTE NORMALLY
		 	}).strength(xStrength))
		.force('y', forceY().y($height / 2).strength(yStrength))
		//.force('collide',forceCollide().radius(d => $rGet(d)*rScale)) //COLLISSION TAKING RADIUS INTO ACCOUNT
		.force('collide',forceCollide().radius(d => width < 400 ? ($rGet(d)*rScale)/1.25 : $rGet(d)*rScale)) //COLLISSION TAKING RADIUS INTO ACCOUNT. if svg width below 400, scale down
		//.restart();
		.stop();
		
		
		//width < 400 ? r / 1.25 : r	


	$: {
		for ( let i = 0,
			n = 120;
			// The REPL thinks there is an infinite loop with this next line but it's generally a better way to go
			//n = Math.ceil(Math.log(simulation.alphaMin()) / Math.log(1 - simulation.alphaDecay()));
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

<!-- if RADIUS scaling no work, put this on line 74 r='{$rGet(node)*rScale}'



<circle
			fill='{$zGet(node)}'
			stroke='{stroke}'
			stroke-width='{strokeWidth}'
			cx='{node.x}'
			cy='{node.y}'
			r='{width < 400 ? ($rGet(node)*rScale) / 1.25 : $rGet(node)*rScale}'
		>
			{#if getTitle}
				<title>{getTitle(node)}</title>
			{/if}
		</circle>


-->