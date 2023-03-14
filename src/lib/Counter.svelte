<script>
  import { onMount } from 'svelte';
  import p5 from 'p5';
  
  let sketch;
  let angle = 0;
  let pitch = 0.5;

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
      let dragStartY;
      let dragDistY;
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
        
      //camera Yscale compression
        let counterCamX = camX * p.cos(angle) - camY * p.sin(angle);
        let counterCamY = camX * p.sin(angle) + camY * p.cos(angle);
        //camera bijschalen
        let scaleCamY = counterCamY * (1-pitch);
        //camera coords terug draaien
        let exitCam = {x: counterCamX * p.cos(angle) + scaleCamY * p.sin(angle), y:counterCamX * -p.sin(angle) + scaleCamY * p.cos(angle)}
        
        ballArray.forEach((item,i) => {
          //item counter rotaten
          item.counter = {x: item.x * p.cos(angle) - item.y * p.sin(angle), y: item.x * p.sin(angle) + item.y * p.cos(angle)}
          //item.y scalen
          item.scale = {y: item.counter.y * (1-pitch)};
          //item terug rotaten
           item.exit = {x: item.counter.x * p.cos(angle) + item.scale.y * p.sin(angle), y: item.counter.x * -p.sin(angle) + item.scale.y * p.cos(angle)};
        })

        ballArray.sort(function(a,b){return a.counter.y - b.counter.y})

        ballArray.forEach((item,i) => {
          p.fill('gray');
          p.push()
       
          p.rotate(angle)

          //item counter rotaten
          let counterX = item.x * p.cos(angle) - item.y * p.sin(angle);
          let counterY = item.x * p.sin(angle) + item.y * p.cos(angle);
          //item.y scalen
          let scaleY = counterY * (1-pitch);
          //item terug rotaten
          let exitX = counterX * p.cos(angle) + scaleY * p.sin(angle);
          let exitY = counterX * -p.sin(angle) + scaleY * p.cos(angle);

          p.translate(exitX ,exitY)
          p.translate(-exitCam.x,-exitCam.y)
          p.push()
          p.rotate(-angle)
          p.ellipse(0,0,item.r,item.r * (1-pitch))
          p.pop()
          p.pop()
        })
 
        ballArray.forEach((item,i) => {
          p.fill('#' + item.colour);
          p.push()
       
          p.rotate(angle)

          //item counter rotaten
          let counterX = item.x * p.cos(angle) - item.y * p.sin(angle);
          let counterY = item.x * p.sin(angle) + item.y * p.cos(angle);
          //item.y scalen
          let scaleY = counterY * (1-pitch);
          //item terug rotaten
          let exitX = counterX * p.cos(angle) + scaleY * p.sin(angle);
          let exitY = counterX * -p.sin(angle) + scaleY * p.cos(angle);

          p.translate(exitX,exitY)
          p.rotate(-angle)
          p.translate(0,-item.z * pitch )
          p.rotate(angle)
          p.ellipse(-exitCam.x,-exitCam.y,item.r,item.r)
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
    camX -= p.cos(angle) * (camSpeed* (1- pitch)) + p.sin(angle) * 0;
    camY -= -p.sin(angle) * (camSpeed* (1- pitch)) + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.RIGHT_ARROW)) {
    camX += p.cos(angle) * (camSpeed* (1- pitch)) + p.sin(angle) * 0;
    camY += -p.sin(angle) * (camSpeed* (1- pitch)) + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.UP_ARROW)) {
    camY -= p.cos(angle) * (camSpeed* (1- pitch)) + -p.sin(angle) * 0;
    camX -= p.sin(angle) * (camSpeed* (1- pitch)) + p.cos(angle) * 0;
  } else if (p.keyIsDown(p.DOWN_ARROW)) {
    camY += p.cos(angle) * (camSpeed* (1- pitch)) + -p.sin(angle) * 0;
    camX += p.sin(angle) * (camSpeed* (1- pitch)) + p.cos(angle) * 0;
  }   

      }

p.mousePressed = () => {
  dragStartX = p.mouseX;
  dragStartY = p.mouseY;
  isDragging = true;
}

p.mouseReleased = () => {
  isDragging = false;
}

p.mouseDragged = () => {
  if (isDragging) {
    let dragDistX = p.mouseX - dragStartX;
    let dragDistY = p.mouseY - dragStartY;
    angle -= dragDistX;
    pitch -= dragDistY / 100;
    dragStartX = p.mouseX;
    dragStartY = p.mouseY;
  }
}
    };
    sketch = new p5(sketchFunc);
  });

  // Balls


</script>

<div id="sketch-container"></div>