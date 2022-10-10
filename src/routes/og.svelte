<script>

    import { tweened } from "svelte/motion";
  
    //import { onMount } from 'svelte';

    export let step = 0;
  
   
  
    let current_Step;
  
    
    // onMount(() => {
  
    //   pymChild = new pym.Child();
  
    //   pymChild.onMessage('set', (d) => {
        
    //     const { index } = JSON.parse(d);
        
    //     current_Step = index;
  
    //     console.log("pymchild has received a message. the index is...");
    //     console.log(index);
  
    //   });
  
    //   console.log("this is onmount firing");
    //})
  
  
    let number = tweened(0);
  
    $: if (step == 0) {
      number.set(100);
    } else if (step == 1) {
      number.set(200);
    } else if (step == 2) {
      number.set(300);
    } 
  
  console.log("this is the script section of the +svelte page");
  
  </script>
  
  <section>
  
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
  
    <div class='my_div'>{Math.round($number)}</div>
    <p>yo</p>
    
  
  
  </section>
  
  <style>
      
      .my_div {
          bottom:0;
          left:0;
         font-size:20em;
      }
  
  
  
  </style>