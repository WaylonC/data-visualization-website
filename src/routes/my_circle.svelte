<script>
import { tweened } from "svelte/motion";

export let step = 0;

export let c_fill;
export let c_stroke;
export let c_strokeWidth;
export let c_cx;
export let c_cy;
export let c_r;
export let c_hisp;


const tweenedX = tweened(c_cx, {
    duration: 600
});

const tweenedY = tweened(c_cy, {
    duration: 600
});


const tweenedOpacity = tweened(0, {
      duration: 1200
});



function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min) + min); // The maximum is exclusive and the minimum is inclusive
}


const dist_r = 100 * Math.sqrt(Math.random());
//const dist_r = 200 * Math.random();

const dist_theta = Math.random() * 2 * Math.PI;



//in this phase, we need to determine if uh....not sure yet actually

$: {
  if (step == 0) {
    tweenedX.set(c_cx);
    tweenedY.set(c_cy);
    tweenedOpacity.set(1);
  }
   if (step == 1) {
     tweenedX.set(c_cx);
     tweenedY.set(c_cy);
   }
   if (step == 2) {
     tweenedX.set(c_cx);
     tweenedY.set(c_cy);
     //need to also set opacity fade in for below-70s....
     tweenedOpacity.set(1);
   }
}




// $: {
//       if (step == 0) {
//         tweenedX.set(600 + dist_r * Math.cos(dist_theta));
//         tweenedY.set(250 + dist_r * Math.sin(dist_theta));

//         if (c_fill == '#fc0') {
//           tweenedOpacity.set(1);
//         }

//       }
//       if (step == 1) {
//         tweenedX.set(c_cx);
//         tweenedY.set(c_cy);
           
//       }
//       if (step == 2) {
//         if (c_fill == '#000') {
//           tweenedOpacity.set(1);
//         }
        
//       }
//     }

  



</script>

<circle
			fill='{c_fill}'
			stroke='{c_stroke}'
			stroke-width='{c_strokeWidth}'
			cx='{$tweenedX}'
			cy='{$tweenedY}'
			r='{c_r}'
      opacity='{$tweenedOpacity}'
/>
		

<!-- 
  <circle
			fill='{c_fill}'
			stroke='{c_stroke}'
			stroke-width='{c_strokeWidth}'
			cx='{$tweenedX}'
			cy='{$tweenedY}'
			r='{c_r}'
      opacity='{$tweenedOpacity}'
/>
-->
