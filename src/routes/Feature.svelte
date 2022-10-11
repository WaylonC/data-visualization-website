<script>
    import { tweened } from "svelte/motion";
    import { interpolateLab } from "d3-interpolate";
    import { rgb } from "d3-color";
    export let featurePath;
    export let initialColor;
    export let futureColor;
    export let step;
  
    const color = tweened(initialColor, {
      duration: 600,
      interpolate: interpolateLab
    });

    $: {
      if (step == 2) {
        color.set(rgb(initialColor));
      }
      if (step == 3) {
        color.set(rgb(initialColor));
      }
      if (step == 4) {
        color.set(rgb(futureColor));
      }
    }



  </script>
  
  <style>
    .provinceShape {
      stroke: #444444;
      stroke-width: 0.1;
    }
  </style>
  
  
  <path
    d={featurePath}
    class="provinceShape"
    fill={$color}
    opacity=50%
      
/>


