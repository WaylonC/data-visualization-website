<script>


    import { onMount } from 'svelte';
  
    import Scatterplot from "./Scatterplot.svelte";
  
   
  
    let current_Step;
  
    
    onMount(() => {

        console.log("this is onmount firing BEFORE THE PYM STUFF");
  
      pymChild = new pym.Child();
  
      pymChild.onMessage('set', (d) => {
        
        const { index } = JSON.parse(d);
        
        current_Step = index;
  
        console.log("pymchild has rreceived a message. the index is...");
        console.log(index);
  
      });
  
       console.log("this is onmount firing AFTER TEH PYM STUFF");
    })
  
  
  console.log("this is the script section of the +svelte page");
  
  </script>
  
  
  
  
  
  <section>
  
  
    <div class="sticky">
      <Scatterplot step={current_Step} />
    </div>
   
    
  
  
  </section>
  
  <style>
  
    :global(body) {
          overflow-x: hidden;
      }
  
  
      
      .sticky {
          position:sticky;
          top:10%;
      flex: 1 1 60%;
      width: 60%;
      }
  
  
  
  </style>