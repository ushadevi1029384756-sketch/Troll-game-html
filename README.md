<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Troll Game - 50 Levels</title>

<style>
body {
  margin: 0;
  background: #111;
  overflow: hidden;
  font-family: Arial;
}
canvas {
  display: block;
  background: #222;
}
#controls {
  position: fixed;
  bottom: 20px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
}
.left {
  display: flex;
  gap: 10px;
}
button {
  width: 70px;
  height: 70px;
  font-size: 22px;
  border: none;
  border-radius: 10px;
  background: #444;
  color: white;
}
button:active { background: #666; }
#msg {
  position: fixed;
  top: 35%;
  width: 100%;
  text-align: center;
  color: white;
  font-size: 28px;
  display: none;
}
</style>
</head>

<body>

<canvas id="game"></canvas>
<div id="msg"></div>

<div id="controls">
  <div class="left">
    <button id="left">◀</button>
    <button id="right">▶</button>
  </div>
  <button id="jump">⬆</button>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");
canvas.width = innerWidth;
canvas.height = innerHeight;

let level = 1;
let maxLevel = 50;
let player, obstacles, dead = false;
let moveLeft = false, moveRight = false;

function startLevel() {
  player = { x: 50, y: 200, w: 30, h: 30, vy: 0, onGround: false };
  obstacles = [];
  dead = false;
  document.getElementById("msg").style.display = "none";

  let count = 6 + level;
  for (let i = 1; i <= count; i++) {
    obstacles.push({
      x: i * 180,
      type: Math.floor(Math.random() * 10),
      active: true
    });
  }
}

startLevel();

document.getElementById("left").ontouchstart = () => moveLeft = true;
document.getElementById("left").ontouchend = () => moveLeft = false;
document.getElementById("right").ontouchstart = () => moveRight = true;
document.getElementById("right").ontouchend = () => moveRight = false;
document.getElementById("jump").ontouchstart = () => {
  if (player.onGround) {
    player.vy = -15;
    player.onGround = false;
  }
};

canvas.onclick = () => {
  if (dead) startLevel();
};

function die() {
  dead = true;
  document.getElementById("msg").innerHTML =
    "💀 YOU DIED<br>Level " + level + "<br>Tap to Retry";
  document.getElementById("msg").style.display = "block";
}

function update() {
  if (dead) return;

  let speed = 3 + level * 0.05;
  if (moveLeft) player.x -= speed;
  if (moveRight) player.x += speed;

  player.vy += 1;
  player.y += player.vy;

  let groundY = canvas.height - 80;
  player.onGround = false;

  obstacles.forEach(o => {
    if (!o.active) return;

    if (Math.abs(player.x - o.x) < 50) {
      switch (o.type) {
        case 0: groundY = canvas.height; break;             // hole
        case 1: die(); break;                               // spikes
        case 2: o.x -= 2 + level * 0.1; break;              // chasing spikes
        case 3: if (player.x > o.x) die(); break;           // wall
        case 4: if (Math.abs(player.x - o.x) < 15) die(); break; // crusher
        case 5: if (player.y > groundY - 20) die(); break;  // fake ground
        case 6: groundY -= 120; break;                      // sudden high ground
        case 7: player.x -= 8; break;                       // speed trap
        case 8: groundY = canvas.height; break;             // invisible pit
        case 9: die(); break;                               // instant troll
      }
    }
  });

  if (player.y + player.h >= groundY) {
    player.y = groundY - player.h;
    player.vy = 0;
    player.onGround = true;
  }

  if (player.y > canvas.height) die();

  if (player.x > obstacles[obstacles.length - 1].x + 100) {
    level++;
    if (level > maxLevel) {
      document.getElementById("msg").innerHTML =
        "🏆 YOU BEAT ALL 50 LEVELS!";
      document.getElementById("msg").style.display = "block";
      dead = true;
    } else {
      startLevel();
    }
  }
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = "#555";
  ctx.fillRect(0, canvas.height - 80, canvas.width, 80);

  ctx.fillStyle = "lime";
  ctx.fillRect(player.x, player.y, player.w, player.h);

  ctx.fillStyle = "red";
  obstacles.forEach(o => {
    ctx.fillRect(o.x, canvas.height - 120, 30, 40);
  });

  ctx.fillStyle = "white";
  ctx.font = "18px Arial";
  ctx.fillText("Level: " + level + " / 50", 20, 30);
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}
loop();
</script>

</body>
</html><!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Troll Game - 50 Levels</title>

<style>
body {
  margin: 0;
  background: #111;
  overflow: hidden;
  font-family: Arial;
}
canvas {
  display: block;
  background: #222;
}
#controls {
  position: fixed;
  bottom: 20px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
}
.left {
  display: flex;
  gap: 10px;
}
button {
  width: 70px;
  height: 70px;
  font-size: 22px;
  border: none;
  border-radius: 10px;
  background: #444;
  color: white;
}
button:active { background: #666; }
#msg {
  position: fixed;
  top: 35%;
  width: 100%;
  text-align: center;
  color: white;
  font-size: 28px;
  display: none;
}
</style>
</head>

<body>

<canvas id="game"></canvas>
<div id="msg"></div>

<div id="controls">
  <div class="left">
    <button id="left">◀</button>
    <button id="right">▶</button>
  </div>
  <button id="jump">⬆</button>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");
canvas.width = innerWidth;
canvas.height = innerHeight;

let level = 1;
let maxLevel = 50;
let player, obstacles, dead = false;
let moveLeft = false, moveRight = false;

function startLevel() {
  player = { x: 50, y: 200, w: 30, h: 30, vy: 0, onGround: false };
  obstacles = [];
  dead = false;
  document.getElementById("msg").style.display = "none";

  let count = 6 + level;
  for (let i = 1; i <= count; i++) {
    obstacles.push({
      x: i * 180,
      type: Math.floor(Math.random() * 10),
      active: true
    });
  }
}

startLevel();

document.getElementById("left").ontouchstart = () => moveLeft = true;
document.getElementById("left").ontouchend = () => moveLeft = false;
document.getElementById("right").ontouchstart = () => moveRight = true;
document.getElementById("right").ontouchend = () => moveRight = false;
document.getElementById("jump").ontouchstart = () => {
  if (player.onGround) {
    player.vy = -15;
    player.onGround = false;
  }
};

canvas.onclick = () => {
  if (dead) startLevel();
};

function die() {
  dead = true;
  document.getElementById("msg").innerHTML =
    "💀 YOU DIED<br>Level " + level + "<br>Tap to Retry";
  document.getElementById("msg").style.display = "block";
}

function update() {
  if (dead) return;

  let speed = 3 + level * 0.05;
  if (moveLeft) player.x -= speed;
  if (moveRight) player.x += speed;

  player.vy += 1;
  player.y += player.vy;

  let groundY = canvas.height - 80;
  player.onGround = false;

  obstacles.forEach(o => {
    if (!o.active) return;

    if (Math.abs(player.x - o.x) < 50) {
      switch (o.type) {
        case 0: groundY = canvas.height; break;             // hole
        case 1: die(); break;                               // spikes
        case 2: o.x -= 2 + level * 0.1; break;              // chasing spikes
        case 3: if (player.x > o.x) die(); break;           // wall
        case 4: if (Math.abs(player.x - o.x) < 15) die(); break; // crusher
        case 5: if (player.y > groundY - 20) die(); break;  // fake ground
        case 6: groundY -= 120; break;                      // sudden high ground
        case 7: player.x -= 8; break;                       // speed trap
        case 8: groundY = canvas.height; break;             // invisible pit
        case 9: die(); break;                               // instant troll
      }
    }
  });

  if (player.y + player.h >= groundY) {
    player.y = groundY - player.h;
    player.vy = 0;
    player.onGround = true;
  }

  if (player.y > canvas.height) die();

  if (player.x > obstacles[obstacles.length - 1].x + 100) {
    level++;
    if (level > maxLevel) {
      document.getElementById("msg").innerHTML =
        "🏆 YOU BEAT ALL 50 LEVELS!";
      document.getElementById("msg").style.display = "block";
      dead = true;
    } else {
      startLevel();
    }
  }
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = "#555";
  ctx.fillRect(0, canvas.height - 80, canvas.width, 80);

  ctx.fillStyle = "lime";
  ctx.fillRect(player.x, player.y, player.w, player.h);

  ctx.fillStyle = "red";
  obstacles.forEach(o => {
    ctx.fillRect(o.x, canvas.height - 120, 30, 40);
  });

  ctx.fillStyle = "white";
  ctx.font = "18px Arial";
  ctx.fillText("Level: " + level + " / 50", 20, 30);
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}
loop();
</script>

</body>
</html>
