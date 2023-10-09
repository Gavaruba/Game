---
layout: default
title: 2D game
permalink: /Game
---

<style>
    #canvas {
        margin: 0;
        border: 1px solid white;
    }
</style>
<canvas id='canvas'></canvas>
<script>
    // Create empty canvas
    let canvas = document.getElementById('canvas');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 720;
    canvas.height = 720;
    // Define the Player class
    class Player {
        constructor() {
            // Initial position and velocity of the player
            this.position = {
                x: 100,
                y: 200
            };
            this.velocity = {
                x: 0,
                y: 0
            };
            // Dimensions of the player
            this.width = 20;
            this.height = 20;
        }
        // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'red';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the players position and velocity
        update() {
            this.draw();
            this.position.y += this.velocity.y;
            this.position.x += this.velocity.x;
        }
    }
    // Create a player object
    player = new Player();
    // Define keyboard keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        },
        up: {
            pressed: false
        },
        down: {
            pressed: false
        }
    };
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        player.update();
        if (keys.right.pressed) {
            player.velocity.x = 5;
        } else if (keys.left.pressed)  {
            player.velocity.x = -5;           
        } else if (keys.up.pressed) {
            player.velocity.y = -5; 
        } else if (keys.down.pressed) {
            player.velocity.y = 5;   
        } 
            else {
            player.velocity.x = 0;
            player.velocity.y = 0;
            }
        //Make player loop through boundaries
        if (player.position.x >= 800) {
            player.position.x = 0;
        }
            else if (player.position.x <= 0) {
            player.position.x = 800;
        }
         if (player.position.y >= 800) {
            player.position.y = 0;
        }
            else if (player.position.y <= 0) {
            player.position.y = 800;
        }
    }
    animate();
    // Event listener for keydown events
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 83:
                console.log('down');
                keys.down.pressed = true;
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                keys.up.pressed = true;
                break;
        }
    });
    // Event listener for keyup events
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                break;
            case 83:
                console.log('down');
                keys.down.pressed = false;
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                keys.up.pressed = false;
                break;
                }
    });
     class GenericObject {
        constructor({ x, y, image }) {
            this.position = {
                x,
                y
            };
            this.image = image;
            this.width = 800;
            this.height = 800;
        }
        // Method to draw the generic object on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y);
        }
    }
    let genericObjects = [
        new GenericObject({
            x:0, y:0, image: imageBackground
        }),
    ];
     let imageBackground = new Image();
     imageBackground.src = 'https://Gavaruba.github.io/Game/2DGame/Images/whiteBlock.jpg';

</script>