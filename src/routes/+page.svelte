<script>

    import { tweened } from "svelte/motion";
  
    import { onMount } from 'svelte';
  
   
  
    let current_Step;
  
    
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
  
  
    let number = tweened(0);
  
    $: if (current_Step == 0) {
      number.set(100);
    } else if (current_Step == 1) {
      number.set(200);
    } else if (current_Step == 2) {
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
  
    <div class='sticky'>{Math.round($number)}</div>
    <p>yo</p>
    
  
  
  </section>
  
  <style>
      
      .sticky {
          position:sticky;
          bottom:0;
          left:0;
      font-size:20em;
      }
  
  
  
  </style>