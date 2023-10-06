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
    // NEW CODE - DEFINE BLOCKOBJECT CLASS
    //--
    class BlockObject {
        constructor(image) {
            // Initial position of the block object
            this.position = {
                x: 100,
                y: 100
            };
            this.image = image;
            this.width = 100;
            this.height = 100;
        }
        // Method to draw the block object on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y);
        }
    }
    // Load images
    let image = new Image();
    //--
    // NEW CODE - ADD IMAGE FOR BLOCK OBJECT
    //--
    let imageBlock = new Image();
    // Load image sources
    image.src = 'https://samayass.github.io/samayaCSA/images/platform.png';
    imageBlock.src = 'https://gavaruba.github.io/Game/2DGame/Images/whiteBlock.jpg';
    //--
    // CREATE BLOCK OBJECT
    //--
    let blockObject = new BlockObject(imageBlock);
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        // Draw platform, player, tube, and block object
        blockObject.draw();
        }
        //--
        // DRAW BLOCK OBJECT
        //--
        blockObject.draw();
        //--
        // COLLISIONS BETWEEN BLOCK OBJECT AND PLAYER
        //--
        if (
            player.position.y + player.height <= blockObject.position.y &&
            player.position.y + player.height + player.velocity.y >= blockObject.position.y &&
            player.position.x + player.width >= blockObject.position.x &&
            player.position.x <= blockObject.position.x + blockObject.width
        )
        {
            player.velocity.y = 0;
        }

</script>