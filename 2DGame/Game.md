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
    (function () {
        const BLOCK = 30;
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 600;
        canvas.height = 600;
        const gridSize = canvas.width / BLOCK;
        let score = 0; // Initialize score
        class Player {
            constructor() {
                this.position = {
                    x: 10,
                    y: 10
                };
                this.velocity = {
                    x: 0,
                    y: 0
                };
                this.radius = 10; // Pac-Man's radius
                this.mouthAngle = 0; // Angle to control Pac-Man's mouth opening
                this.direction = 'right'; // Initial direction
            }
            draw() {
                ctx.fillStyle = 'yellow';
                ctx.beginPath();
                if (this.direction === 'right') {
                    ctx.arc(this.position.x, this.position.y, this.radius, (0 + this.mouthAngle) * Math.PI, (2 - this.mouthAngle) * Math.PI);
                } else {
                    ctx.arc(this.position.x, this.position.y, this.radius, (2 + this.mouthAngle) * Math.PI, (0 - this.mouthAngle) * Math.PI);
                }
                ctx.lineTo(this.position.x, this.position.y);
                ctx.fill();
            }
            update() {
                this.draw();
                // Update mouth animation
                if (this.direction === 'right') {
                    this.mouthAngle += 0.02;
                    if (this.mouthAngle > 0.5) this.direction = 'left';
                } else {
                    this.mouthAngle -= 0.02;
                    if (this.mouthAngle < 0) this.direction = 'right';
                }
                // Update player's position
                this.position.x += this.velocity.x;
                this.position.y += this.velocity.y;
            }
        }
        const player = new Player();
        const keys = {
            right: { pressed: false },
            left: { pressed: false },
            up: { pressed: false },
            down: { pressed: false }
        };
        class Food {
            constructor() {
                this.position = {
                    x: Math.floor(Math.random() * gridSize),
                    y: Math.floor(Math.random() * gridSize)
                };
                this.radius = 10;
            }
            draw() {
                ctx.fillStyle = 'white';
                ctx.beginPath();
                ctx.lineTo(this.position.x, this.position.y);
                ctx.fill();
            }
            update() {
                this.draw();
            }
        }
        function animate() {
            requestAnimationFrame(animate);
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            player.update();
            }
        // Function to check if Pac-Man eats the food
        function eatFood() {
            if (
                (player.position.x - food.x) < 10 &&
                (player.position.y - food.y) < 10
            ) {
        // Increase the score and generate new food
                score += 10;
                document.getElementById('score').innerText = `Score: ${score}`;
                food = {
                    x: Math.floor(Math.random() * gridSize),
                    y: Math.floor(Math.random() * gridSize)
                };
            }
        }
        animate();
        addEventListener('keydown', ({ keyCode }) => {
            switch (keyCode) {
                case 37:
                    // Left key
                    player.velocity.x = -1;
                    player.velocity.y = 0;
                    break;
                case 38:
                    // Up key
                    player.velocity.x = 0;
                    player.velocity.y = -1;
                    break;
                case 39:
                    // Right key
                    player.velocity.x = 1;
                    player.velocity.y = 0;
                    break;
                case 40:
                    // Down key
                    player.velocity.x = 0;
                    player.velocity.y = 1;
                    break;
            }
            eatFood();
        });
        addEventListener('keyup', ({ keyCode }) => {
            switch (keyCode) {
                case 37:
                case 38:
                case 39:
                case 40:
                    player.velocity.x = 0;
                    player.velocity.y = 0;
                    break;
            }
        });
        player.draw();
    })();
</script>
<p id="score">Score: 0</p>