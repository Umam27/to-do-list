### This is a fun CSS effect that follows your mouse around. It could be useful when you want to draw attention to an element on your page.

It's a great effect to start with because ee need very little HTML:

```
<div class="demo">
  <div class="perspective-container">
    <div class="card"></div>
  </div>
</div>
```

First, we position the demo and set perspective for our 3D transform:
```

.demo {
  background-color: hsl(207, 9%, 19%);
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100%;
}

.perspective-container {
  perspective: 800px;
}
```

Then style the div we want to animate:
```
.card {
  background-image: url(https://media.giphy.com/media/sIIhZliB2McAo/giphy.gif);
  background-size: cover;
  box-shadow: 0 0 140px 10px rgba(0,0,0,.5);
  position: relative;
  height: 300px;
  width: 500px;
  overflow: hidden; /* Try removing this to see how the sheen works! */
  --sheenX: 0; /* Set these with JavaScript */
  --sheenY: 0;
}
```

Here we set a background, then we set overflow to hidden so that we can add a sheen effect later. We also set css variables, sheenX and sheenY.

These sheen variables will help position the sheen effect. We use them in our card's after pseudo-element:
```
.card::after {
  content: "";
  position: absolute;
  top: -400px;
  right: -400px;
  bottom: -400px;
  left: -400px;
  background: linear-gradient(217deg, rgba(255,255,255,0), rgba(255,255,255,0) 35%, rgba(255,255,255,0.25) 45%, rgba(255,255,255,.25) 50%, rgba(255,255,255,0) 60%, rgba(255,255,255,0) 100%);
  transform: translateX(var(--sheenX)) translateY(var(--sheenY));
}
```
Here we're making sure the pseudo-element is bigger than the container. This will give us something to slide around on top of the card using transform.

The transform property is making use of those CSS variables we set earlier. We will set those with JavaScript. Let's set up the JavaScript to first listen for mouse events:

document.onmousemove = handleMouseMove;

We now need a handleMouseMove function to handle onmousemove:
```
function handleMouseMove(event) {
  const height = window.innerHeight;
  const width = window.innerWidth;
  // Creates angles of (-20, -20) (left, bottom) and (20, 20) (right, top)
  const yAxisDegree = event.pageX / width * 40 - 20;
  const xAxisDegree = event.pageY / height * -1 * 40 + 20;
  target.style.transform = `rotateY(${yAxisDegree}deg) rotateX(${xAxisDegree}deg)`;
  // Set the sheen position
  setSheenPosition(event.pageX / width, event.pageY / width);
}
```

Our function takes the window height and width and creates an angle on the X and Y axes. We then set these to the transform style of our card. This gives the card an angle based on the mouse!

We next call a function to set the pseudo-element's position:
```
function setSheenPosition(xRatio, yRatio) {
  // This creates a "distance" up to 400px each direction to offset the sheen
  const xOffset = 1 - (xRatio - 0.5) * 800;
  const yOffset = 1 - (yRatio - 0.5) * 800;
  target.style.setProperty('--sheenX', `${xOffset}px`)
  target.style.setProperty('--sheenY', `${yOffset}px`)
}
```
Our pseudo-element looks best when it moves in the opposite direction to the mouse. To achieve this we create a number between -0.5 and 0.5 that changes in the opposite direction by calculating the ratio by -1.

We multiply this number by 800 as we want it to scale up to a maximum of 400px, which is how far we set the sheen pseudo-element outside the card.

Lastly we set these offset values to our CSS variable properties, and the browser's renderer does the rest.

We now have a card that turns to face our mouse while the sheen effect moves in the opposite direction on top. This creates a nice, eye-catching effect.
