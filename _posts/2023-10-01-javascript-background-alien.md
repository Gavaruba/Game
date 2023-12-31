---
layout: default
title: Pacman Background
description: Use JavaScript without external libraries to loop background moving across screen, OOP style.
categories: [C5.0, C7.0, C7.6]
image: /images/game_background.jpg
type: tangibles
courses: { compsci: {week: 1} }
---

{% assign gameBackgroundFile = site.baseurl | append: page.image %}
{% assign WIDTH = 7000 %}
{% assign HEIGHT = 2500 %}

<canvas id="game"></canvas>

<script>
  const canvas = document.getElementById("Pacman");
  const ctx = canvas.getContext('2d');

  const ASPECT_RATIO = {{WIDTH}} / {{HEIGHT}};
  const maxWidth = window.innerWidth;
  const maxHeight = window.innerHeight;

  // Set Dimensions to match the image width
  const canvasWidth = {{WIDTH}};
  const canvasHeight = canvasWidth / ASPECT_RATIO;
  const canvasLeft = 0; // Start from the left edge
  const canvasTop = (maxHeight - canvasHeight) / 2;

  // Set Style properties
  canvas.width = canvasWidth;
  canvas.height = canvasHeight;
  canvas.style.width = `${canvasWidth}px`;
  canvas.style.height = `${canvasHeight}px`;

  canvas.style.position = 'absolute';
  canvas.style.left = `${canvasLeft}px`;
  canvas.style.top = `${canvasTop}px`;

  var gameSpeed = 5;
  class Layer {
    constructor(image, speedRatio) {
      this.x = 0;
      this.y = 0;
      this.width = {{WIDTH}};
      this.height ={{HEIGHT}};
      this.image = image
      this.speedRatio = speedRatio
      this.speed = gameSpeed * this.speedRatio;
      this.frame = 0;
    }
    update() {
      this.x = (this.x - this.speed) % this.width;
    }
    draw(){
      ctx.drawImage(this.image, this.x, this.y);
    }
  }

  const backgroundImg = new Image();
  backgroundImg.src = '{{gameBackgroundFile}}';
  var backgroundObj = new Layer(backgroundImg, 0.5)

  function background() {
    backgroundObj.update();
    backgroundObj.draw();
    requestAnimationFrame(background);
  }
  background();

</script>
