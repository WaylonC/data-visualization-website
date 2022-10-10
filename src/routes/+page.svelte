<script>

	
  import { onMount } from 'svelte';
  import Map from "./Component_Map.svelte";
  import MyBeeSwarm from "./Component_Beeswarm.svelte";
  import OG from "./og.svelte";
    
  //let value;
  let current_Step = 0;

  //diff components
  const options = [
		{ name: 'og',component: OG},
		{ name: 'map',component: Map},
		{ name: 'beeswarm',component: MyBeeSwarm}
	];
  let selected = options[2];
  

  //comment the onmoutn thing below when you want to switch to in-svelte scrolly
  onMount(() => {

    pymChild = new pym.Child();

    pymChild.onMessage('set', (d) => {
  
      const { index } = JSON.parse(d);
  
      current_Step = index;
      selected = options[current_Step];

      console.log("pymchild has received a message. the index is...");
      console.log(index);

    });

  console.log("this is onmount firing");

    


  })

  $: {console.log(selected)};

</script>

<section>

  <select bind:value={selected}>
    {#each options as option}
      <option value={option}>yep</option>
    {/each}
  </select>
    

  <svelte:component this={selected.component}/>

<!-- <Map step={current_Step} /> -->


<!-- <MyBeeSwarm step={current_Step} /> -->

<!-- <OG step={current_Step}/> --> 

</section>

<style>
    :global(body) {
        /*overflow-x: hidden; */ /*this freaks out when used with the second_svelte map, but was important previously with the d-3 map*/
    }
    
  /* importn */

  .sticky {
    position: sticky;
    top: 10%;
        /*flex: 1 1 60%;
    width: 70%;*/
  }


</style>








    <!-- <option value=0> 0 </option>
    <option value=1> 1 </option>
    <option value=2> 2 </option>
    <option value=3> 3 </option>
    <option value=4> 4 </option>
    <option value=5> 5 </option>
    <option value=6> 6 </option> -->




  


  

