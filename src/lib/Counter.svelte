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
        
        p.translate(-p.width / 2,-p.height / 2)
        p.fill('red')
        p.ellipse(p.width / 2,p.height / 2,10,10)
      
        
    
        p.rotate(angle)
        p.translate(camX,camY)


        
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

        ballArray.forEach((item,i) => {
          p.push()
          //draw shadow
          p.fill('gray');
        
          p.translate(item.x,item.y);
          p.rotate(-angle);
          p.translate(p.width / 2 - camX, p.height / 2 - camY);

          p.ellipse(0,0,item.r,item.r);
        p.pop()
        })

        ballArray.forEach((item,i) => {
          p.push()
          //draw shadow
         p.translate(item.x,item.y);
          p.rotate(-angle);
          p.translate(p.width / 2 - camX, p.height / 2 - camY);
          p.fill('#' + item.colour);
          p.ellipse(0,-item.z,item.r,item.r)
        p.pop()
        })  
        
        if (p.keyIsDown(p.LEFT_ARROW)) {
    camX += camSpeed;
  } else if (p.keyIsDown(p.RIGHT_ARROW)) {
    camX -= camSpeed;
  } else if (p.keyIsDown(p.UP_ARROW)) {
    camY += camSpeed;
  } else if (p.keyIsDown(p.DOWN_ARROW)) {
    camY -= camSpeed;
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