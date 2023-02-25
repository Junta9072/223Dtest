<script>
  import { onMount } from 'svelte';
  import p5 from 'p5';
  
  let sketch;

  let ballArray = []
  for (let i = 0; i < 20; i++) {
      ballArray.push({x: Math.random() * 400, y: Math.random() * 400, z: Math.random() * 100, r: Math.random() * 40 + 40, colour: Math.floor(Math.random()*16777215).toString(16)})
}

ballArray.sort(function(a,b){return a.y - b.y})

  onMount(() => {
    const sketchFunc = (p) => {
      // Your p5.js sketch code goes here
      p.setup = () => {
        p.createCanvas(400, 400);
        p.background(200);
      };

    
      p.draw = () => {
        p.noStroke();
        ballArray.forEach((item, i) => {
          p.fill('gray');
          p.ellipse(item.x, item.y, item.r, item.r);
        })

        ballArray.forEach((item, i) => {
          p.fill("#" + item.colour);
          p.ellipse(item.x, item.y - item.z, item.r, item.r);
        })
      };
    };

    sketch = new p5(sketchFunc);
  });

  // Balls


</script>

<div id="sketch-container"></div>