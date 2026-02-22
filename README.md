<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Snake Game</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background: #222;
      margin: 0;
      color: #0f0;
      font-family: Arial, sans-serif;
    }
    canvas {
      background: #111;
      border: 2px solid #0f0;
    }
    #score {
      font-size: 20px;
      margin: 10px;
    }
    #restartBtn {
      margin-top: 10px;
      padding: 8px 16px;
      background: #0f0;
      border: none;
      cursor: pointer;
      font-weight: bold;
    }
    #restartBtn:hover {
      background: #1f1;
    }
  </style>
</head>
<body>
  <div id="score">Score: 0</div>
  <canvas id="gameCanvas" width="400" height="400"></canvas>
  <button id="restartBtn">Restart Game</button>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const scoreDisplay = document.getElementById("score");
    const restartBtn = document.getElementById("restartBtn");

    const box = 20;
    let snake, direction, food, score, game;

    function init() {
      snake = [{ x: 9 * box, y: 10 * box }];
      direction = "RIGHT";
      food = {
        x: Math.floor(Math.random() * 19 + 1) * box,
        y: Math.floor(Math.random() * 19 + 1) * box
      };
      score = 0;
      scoreDisplay.textContent = "Score: " + score;
      clearInterval(game);
      game = setInterval(draw, 100);
    }

    document.addEventListener("keydown", changeDirection);
    restartBtn.addEventListener("click", init);

    function changeDirection(event) {
      if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
      else if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
      else if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
      else if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // খাবার আঁকা
      ctx.fillStyle = "red";
      ctx.fillRect(food.x, food.y, box, box);

      // স্নেক আঁকা
      for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? "lime" : "green";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
      }

      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if (direction === "LEFT") snakeX -= box;
      if (direction === "UP") snakeY -= box;
      if (direction === "RIGHT") snakeX += box;
      if (direction === "DOWN") snakeY += box;

      // খাবার খাওয়া
      if (snakeX === food.x && snakeY === food.y) {
        score++;
        scoreDisplay.textContent = "Score: " + score;
        food = {
          x: Math.floor(Math.random() * 19 + 1) * box,
          y: Math.floor(Math.random() * 19 + 1) * box
        };
      } else {
        snake.pop();
      }

      let newHead = { x: snakeX, y: snakeY };

      // গেম ওভার শর্ত
      if (
        snakeX < 0 || snakeY < 0 ||
        snakeX >= canvas.width || snakeY >= canvas.height ||
        collision(newHead, snake)
      ) {
        clearInterval(game);
        alert("Game Over! Final Score: " + score);
      }

      snake.unshift(newHead);
    }

    function collision(head, array) {
      for (let i = 0; i < array.length; i++) {
        if (head.x === array[i].x && head.y === array[i].y) {
          return true;
        }
      }
      return false;
    }

    init();
  </script>
</body>
</html>
