<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Juego - Hombre Salta Obstáculos</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="game-container">
    <div id="player" class="player"></div>
    <div id="obstacle" class="obstacle"></div>
    <h1 id="score">Puntuación: 0</h1>
  </div>

  <script src="script.js"></script>
</body>
</html>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  background-color: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  overflow: hidden;
}

.game-container {
  position: relative;
  width: 800px;
  height: 400px;
  background-color: #d6eaf8;
  border: 2px solid #2980b9;
  overflow: hidden;
}

.player {
  position: absolute;
  bottom: 0;
  left: 50px;
  width: 50px;
  height: 100px;
  background-color: #fff;
  border: 2px solid black;
  border-radius: 10px;
  background-image: url('man-character.png'); /* Reemplaza con una imagen si quieres */
  background-size: cover;
}

.obstacle {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 50px;
  height: 50px;
  background-color: #8e44ad;
  border-radius: 5px;
}

#score {
  position: absolute;
  top: 10px;
  left: 10px;
  font-size: 20px;
}
const player = document.getElementById("player");
const obstacle = document.getElementById("obstacle");
const scoreDisplay = document.getElementById("score");

let isJumping = false;
let score = 0;

// Movimiento del jugador (salto)
document.addEventListener("keydown", (e) => {
  if (e.code === "Space" && !isJumping) {
    jump();
  }
});

function jump() {
  isJumping = true;
  let jumpHeight = 0;
  let jumpInterval = setInterval(() => {
    if (jumpHeight >= 150) {
      clearInterval(jumpInterval);
      let fallInterval = setInterval(() => {
        if (jumpHeight <= 0) {
          clearInterval(fallInterval);
          isJumping = false;
        }
        jumpHeight -= 5;
        player.style.bottom = `${jumpHeight}px`;
      }, 20);
    }
    jumpHeight += 5;
    player.style.bottom = `${jumpHeight}px`;
  }, 20);
}

// Movimiento del obstáculo
function moveObstacle() {
  let obstaclePosition = 800;
  const randomObstacle = getRandomObstacle();

  const obstacleInterval = setInterval(() => {
    if (obstaclePosition <= 0) {
      obstaclePosition = 800;
      score++;
      scoreDisplay.textContent = `Puntuación: ${score}`;
    }

    if (obstaclePosition <= 100 && obstaclePosition >= 50 && parseInt(player.style.bottom) < 50) {
      alert("¡Perdiste! Puntuación final: " + score);
      score = 0;
      scoreDisplay.textContent = `Puntuación: ${score}`;
      obstaclePosition = 800;
    }

    obstaclePosition -= 5;
    obstacle.style.left = `${obstaclePosition}px`;
  }, 20);
}

// Genera un obstáculo aleatorio (libro, mesa o mochila)
function getRandomObstacle() {
  const obstacles = ["#e74c3c", "#3498db", "#f1c40f"]; // Colores para libros, mesas y mochilas
  const randomColor = obstacles[Math.floor(Math.random() * obstacles.length)];
  obstacle.style.backgroundColor = randomColor;
  return randomColor;
}

// Inicia el movimiento del obstáculo
moveObstacle();
