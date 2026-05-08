<!DOCTYPE html>
<html>
<head>
    <title>Geo Dash Lite</title>
    <style>
        body { margin: 0; background: #222; overflow: hidden; display: flex; align-items: center; justify-content: center; height: 100vh; }
        canvas { background: #00b4ff; border: 4px solid #fff; box-shadow: 0 0 20px rgba(0,0,0,0.5); }
    </style>
</head>
<body>
    <canvas id="game"></canvas>

<script>
const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');

canvas.width = 800;
canvas.height = 400;

// Game Constants
const GRAVITY = 0.8;
const JUMP_STRENGTH = -12;
const SPEED = 5;

let player = {
    x: 100,
    y: 300,
    size: 40,
    dy: 0,
    jumped: false,
    rotation: 0
};

let obstacles = [];
let frame = 0;
let score = 0;
let isGameOver = false;

// Input Handling
window.addEventListener('keydown', (e) => { if (e.code === 'Space') jump(); });
window.addEventListener('mousedown', jump);

function jump() {
    if (!player.jumped) {
        player.dy = JUMP_STRENGTH;
        player.jumped = true;
    }
    if (isGameOver) reset();
}

function reset() {
    player.y = 300;
    player.dy = 0;
    obstacles = [];
    score = 0;
    isGameOver = false;
    animate();
}

function spawnObstacle() {
    if (frame % 100 === 0) {
        obstacles.push({ x: canvas.width, y: 350, w: 40, h: 50 });
    }
}

function update() {
    if (isGameOver) return;

    // Physics
    player.dy += GRAVITY;
    player.y += player.dy;

    // Floor Collision
    if (player.y > 350 - player.size) {
        player.y = 350 - player.size;
        player.dy = 0;
        player.jumped = false;
        // Snap rotation to 90deg increments when landing
        player.rotation = Math.round(player.rotation / 90) * 90;
    } else {
        // Rotate while in air
        player.rotation += 5;
    }

    // Obstacle Logic
    obstacles.forEach((obs, index) => {
        obs.x -= SPEED;

        // Collision Detection (AABB)
        if (player.x < obs.x + obs.w &&
            player.x + player.size > obs.x &&
            player.y < obs.y + obs.h &&
            player.y + player.size > obs.y) {
            isGameOver = true;
        }

        if (obs.x + obs.w < 0) {
            obstacles.splice(index, 1);
            score++;
        }
    });

    frame++;
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draw Floor
    ctx.fillStyle = '#111';
    ctx.fillRect(0, 350, canvas.width, 50);

    // Draw Player with Rotation
    ctx.save();
    ctx.translate(player.x + player.size/2, player.y + player.size/2);
    ctx.rotate(player.rotation * Math.PI / 180);
    ctx.fillStyle = '#ffff00';
    ctx.fillRect(-player.size/2, -player.size/2, player.size, player.size);
    ctx.strokeStyle = '#000';
    ctx.strokeRect(-player.size/2, -player.size/2, player.size, player.size);
    ctx.restore();

    // Draw Obstacles
    ctx.fillStyle = '#ff4444';
    obstacles.forEach(obs => {
        ctx.fillRect(obs.x, obs.y, obs.w, obs.h);
    });

    // Score
    ctx.fillStyle = '#fff';
    ctx.font = '20px Arial';
    ctx.fillText(`Score: ${score}`, 20, 30);

    if (isGameOver) {
        ctx.fillText("GAME OVER - Click to Restart", canvas.width/2 - 120, canvas.height/2);
    }
}

function animate() {
    update();
    draw();
    if (!isGameOver) requestAnimationFrame(animate);
}

animate();
</script>
</body>
</html>
