<script>
  import { onMount } from 'svelte';
  import p5 from 'p5';
  
  let sketch;
  let angle = 0;
   let area = 400;
   let amount = 20;

  let ballArray = []
  for (let i = 0; i < amount; i++) {
    //setup ball props
   
      ballArray.push({x: Math.random() * area - area /2, y: Math.random() * area - area /2, z: Math.random()* 50, r: 60, colour: Math.floor(Math.random()*16777215).toString(16)})
}

ballArray.sort(function(a,b){return a.y - b.y})

  onMount(() => {
    const sketchFunc = (p) => {
      // Your p5.js sketch code goes here
      
      p.setup = () => {
        p.angleMode(p.DEGREES);
        p.noStroke()
        p.createCanvas(400, 400, p.WEBGL);
        p.background(200); 
        p.camera(0, 0, -1000);
        
      };

    
      p.draw = () => {
        // p.fill(255)
        // p.rectMode(p.CENTER)       
        // p.rect(0,0,p.width,p.height);
        p.clear()

        p.angleMode(p.DEGREES);
        p.noStroke()
        
        p.rotate(angle);
        //shadow trick example
          //save canvas state
          p.push();
          //move origin to start of the line
          p.translate(50,50)
          //counteract rotation of the general canvas
          p.rotate(-angle)
          //draw line
          p.stroke('red');
          p.line(0,0,100,100);
          //restore original canvas state
        p.pop()
        p.noStroke()

        //z-sorting

        ballArray.forEach((item,i) => {
          item.rectY = p.sin(angle) * item.x + p.cos(angle) * item.y + p.height/2;
        })
        ballArray.sort(function(a,b) {return a.rectY-b.rectY})
        console.log(ballArray[0].colour,ballArray[1].colour)
        ballArray.forEach((item,i) => {
          p.push()
          //draw shadow
          p.fill('gray');
          p.translate(item.x,item.y);
          p.rotate(-angle);
          p.ellipse(0,item.z,item.r,item.r);
          //draw ball
          p.fill('#' + item.colour);
          p.ellipse(0,0,item.r,item.r)
        p.pop()
        })        
     
        angle+=0.5;
      }
    };

    

    sketch = new p5(sketchFunc);
  });

  // Balls


</script>

<div id="sketch-container"></div>