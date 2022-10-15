<script>

	
  import { onMount } from 'svelte';
  import Map from "./Component_Map.svelte";
  import MyBeeSwarm from "./Component_Beeswarm.svelte";
  import OG from "./og.svelte";
    
  //let value;
  let current_Step = 0;

  //diff components
  //this is also where you set the order of them
  const BeeSwarmStep = 3;

  const options = [
		//{ c_step: 0, component: OG},
		{ c_step: 0, component: Map},
		{ c_step: BeeSwarmStep, component: MyBeeSwarm}
	];
  let selected = options[0];
  

  //comment the onmoutn thing below when you want to switch to in-svelte scrolly
  onMount(() => {

    pymChild = new pym.Child();

    pymChild.onMessage('set', (d) => {
  
      const { index } = JSON.parse(d);
  
      current_Step = index;


      // //switching between da components
      // if (current_Step > 1) {
      //   selected = options[1];
      // }
      // else if (current_Step > 4 ) {
      //   selected = options[2];
      // }
      
      //selected = options[current_Step];

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

  <!-- 
  <select bind:value={current_Step}>
    <option value=0> 0 </option>
    <option value=1> 1 </option>
    <option value=2> 2 </option>
    <option value=3> 3 </option>
    <option value=4> 4 </option>
    <option value=5> 5 </option>
    <option value=6> 6 </option> 
    <option value=7> 7 </option> 
  </select> -->
    

 <svelte:component this={selected.component} step={current_Step - selected.c_step}/>


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




  


  

