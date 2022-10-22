<script>

	
  import { onMount } from 'svelte';
  import Map from "../lib/map/Component_Map.svelte";
  import MyBeeSwarm from "../lib/beeswarm/Component_Beeswarm.svelte";
  import OG from "./og.svelte";
  import { fade } from 'svelte/transition';
    
  //let value;
  let current_Step = 0;

  //diff components
  //this is also where you set the order of them
  const BeeSwarmStep = 5;

  const options = [
		{ c_step: 0, component: Map},
		{ c_step: BeeSwarmStep, component: MyBeeSwarm}
	];
  let selected = options[0];

  let w, h;
  

  //comment the onmoutn thing below when you want to switch to in-svelte scrolly
  onMount(() => {

    pymChild = new pym.Child();

    pymChild.onMessage('set', (d) => {
  
      const { index } = JSON.parse(d);
  
      current_Step = index;


      console.log("pymchild has received a message. the index is...");
      console.log(index);

    });

  console.log("this is onmount firing");

    


  })


  $: {
      if (current_Step >= BeeSwarmStep) {  //honestly might be a good idea to make these into variables
        selected = options[1];
      }
      else if (current_Step < BeeSwarmStep ) {
        selected = options[0];
      }
  };






</script>

<section>

  
  <!-- <select bind:value={current_Step}>
    <option value=0> 0 </option>
    <option value=1> 1 </option>
    <option value=2> 2 </option>
    <option value=3> 3 </option>
    <option value=4> 4 </option>
    <option value=5> 5 </option>
    <option value=6> 6 </option> 
    <option value=7> 7 </option> 
  </select>  -->
  

  <div id="viewport" transition:fade bind:clientWidth={w} bind:clientHeight={h}> <!-- not working atm-->
    <svelte:component this={selected.component} step={current_Step - selected.c_step} w={w} h={h} />
  </div>





</section>










  


  

