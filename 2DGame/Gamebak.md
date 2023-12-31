---
layout: default
title: 2D game
permalink: /Gamebak
---

<style>
    #canvas {
        margin: 0;
        border: 2px solid white;
    }

</style>
<canvas id='canvas'></canvas>
<script>
     ( function () {
     const BLOCK = 30;
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 600;
        canvas.height = 600;
        const gridSize = canvas.width / BLOCK;
        let score = 0;
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
, this.radius, (2 + this.mouthAngle) * Math.PI, (0 - this.mouthAngle) * Math.PI);
                ctx.fillStyle = 'yellow';
                ctx.beginPath();
                if (this.direction === 'right') {
                    ctx.arc(this.position.x, this.position.y, this.radius, (0 + this.mouthAngle) * Math.PI, (2 - this.mouthAngle) * Math.PI);
                } else {
                    ctx.arc(this.position.x, this.position.y)
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
            constructor(x, y) {
                this.position = {
                    x: x,
                    y: y
                };
            this.radius = 5;
            }
            draw() {
                ctx.fillStyle = 'white';
                ctx.beginPath();
                ctx.arc((this.position.x + 0.5) * BLOCK, (this.position.y + 0.5) * BLOCK, this.radius, 0, 2 * Math.PI);
                ctx.fill();
            }
        }
        const foods = [];
        for (let i = 0; i < 15; i++) {
            foods.push(new Food(i, 5));
        }
        // Function to check if Pac-Man eats the food
        function eatFood() {
            for (let i = 0; i < foods.length; i++) {
                const food = foods[i];
                if (Math.abs(player.position.x - food.position.x * BLOCK) < 15 && Math.abs(player.position.y - food.position.y * BLOCK) < 20) {
                    // Increase the score and remove the eaten food
                    score += 10;
                    document.getElementById('score').innerText = `Score: ${score}`;
                    foods.splice(i, 1);
                }
            }
        }
        function animate() {
            requestAnimationFrame(animate);
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (const food of foods) {
                food.draw();
            }
            player.update();
            eatFood();
        }
        animate();
        addEventListener('keydown', ({ keyCode }) => {
            switch (keyCode) {
                case 65:
                    // Left key
                    player.velocity.x = -1;
                    player.velocity.y = 0;
                    break;
                case 87:
                    // Up key
                    player.velocity.x = 0;
                    player.velocity.y = -1;
                    break;
                case 68:
                    // Right key
                    player.velocity.x = 1;
                    player.velocity.y = 0;
                    break;
                case 83:
                    // Down key
                    player.velocity.x = 0;
                    player.velocity.y = 1;
                    break;
            }
        });
        addEventListener('keyup', ({ keyCode }) => {
            switch (keyCode) {
                case 65:
                case 87:
                case 83:
                case 68:
                    player.velocity.x = 0;
                    player.velocity.y = 0;
                    break;
            }
        });
        player.draw();
    ();
</script>
<p id="score">Score: 0</p>