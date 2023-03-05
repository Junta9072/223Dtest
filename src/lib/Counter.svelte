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
   
      ballArray.push({x: Math.random() * area - area /2, y: Math.random() * area - area /2, z: Math.random()* 100, r: Math.random() * 30 + 30, colour: Math.floor(Math.random()*16777215).toString(16)})
}

ballArray.sort(function(a,b){return a.y - b.y})

  onMount(() => {
    const sketchFunc = (p) => {
      // Your p5.js sketch code goes here
      let dragStartX;
      let isDragging = false;

      let camX = 0;
      let camY = 0;

      let angX = 0;
      let angY = 0;
      let camSpeed = 5;
      
      
      p.setup = () => {
        p.angleMode(p.DEGREES);
        p.noStroke()
        p.createCanvas(400, 400, p.WEBGL);
        p.background(200); 
               
      };

    
      p.draw = () => {
        // p.fill(255)
        // p.rectMode(p.CENTER)       
        // p.rect(0,0,p.width,p.height);
        p.clear()

        p.angleMode(p.DEGREES);
        p.noStroke()
           
        p.fill('red')
        p.ellipse(0,0,10,10)        

        ballArray.forEach((item,i) => {
          p.fill('#' + item.colour);
          p.push()
       
          p.rotate(angle)
          p.translate(item.x,item.y)

          p.ellipse(-camX,-camY,item.r,item.r)
          p.pop()
        })

        /*ballArray.forEach((item,i) => {
          p.fill('#' + item.colour);
          p.push();
          p.translate(item.x, item.y);
          p.rotate(-angle)
          p.ellipse(0,-item.z,item.r,item.r)
          p.pop()
        })*/
        
        if (p.keyIsDown(p.LEFT_ARROW)) {
    camX -= p.cos(angle) * camSpeed + p.sin(angle) * 0;
    camY -= -p.sin(angle) * camSpeed + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.RIGHT_ARROW)) {
    camX += p.cos(angle) * camSpeed + p.sin(angle) * 0;
    camY += -p.sin(angle) * camSpeed + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.UP_ARROW)) {
    camY -= p.cos(angle) * camSpeed + -p.sin(angle) * 0;
    camX -= p.sin(angle) * camSpeed + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.DOWN_ARROW)) {
    camY += p.cos(angle) * camSpeed + -p.sin(angle) * 0;
    camX += p.sin(angle) * camSpeed + p.cos(angle) * 0;
  }   

      }

p.mousePressed = () => {
  dragStartX = p.mouseX;
  isDragging = true;
}

p.mouseReleased = () => {
  isDragging = false;
}

p.mouseDragged = () => {
  if (isDragging) {
    let dragDistX = p.mouseX - dragStartX;
    angle -= dragDistX;
    dragStartX = p.mouseX;
  }
}
    };
    sketch = new p5(sketchFunc);
  });

  // Balls


</script>

<div id="sketch-container"></div>