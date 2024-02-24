<!DOCTYPE html>
<html>
<head>
  <title>Snake spil</title>
  <style>
    canvas {
      border: 1px solid black;
      background-color: #f0f0f0;
    }
    #restartButton {
      display: none;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="400" height="400"></canvas>
  <button id="restartButton" onclick="restartGame()">Genstart spillet</button>
  <script>
    var canvas = document.getElementById("gameCanvas");
    var ctx = canvas.getContext("2d");

    var snake = [{x: 200, y: 200}];
    var dx = 10;
    var dy = 0;
    var blockSize = 10;
    var food = {x: 0, y: 0};
    var interval;

    function startGame() {
      interval = setInterval(draw, 100);
      createFood();
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Tjek om slangen har ramt kanten
      if (snake[0].x < 0 || snake[0].x >= canvas.width || snake[0].y < 0 || snake[0].y >= canvas.height) {
        // Hvis slangen rammer kanten, stop spillet
        clearInterval(interval);
        document.getElementById("restartButton").style.display = "block";
        return;
      }

      // Tjek om slangen har spist maden
      if (snake[0].x === food.x && snake[0].y === food.y) {
        // Hvis slangen spiser maden, vokser den og ny mad skabes
        snake.unshift({x: food.x, y: food.y});
        createFood();
      }

      // Tegn maden
      ctx.fillStyle = "red";
      ctx.fillRect(food.x, food.y, blockSize, blockSize);

      // Tegn slangen
      ctx.fillStyle = "green";
      snake.forEach(function(segment) {
        ctx.fillRect(segment.x, segment.y, blockSize, blockSize);
      });

      // Flyt slangen
      var head = {x: snake[0].x + dx, y: snake[0].y + dy};
      snake.unshift(head);
      snake.pop();
    }

    function createFood() {
      // Generer tilfældige koordinater for maden
      food.x = Math.floor(Math.random() * (canvas.width / blockSize)) * blockSize;
      food.y = Math.floor(Math.random() * (canvas.height / blockSize)) * blockSize;
    }

    function restartGame() {
      snake = [{x: 200, y: 200}];
      dx = 10;
      dy = 0;
      document.getElementById("restartButton").style.display = "none";
      startGame();
    }

    document.addEventListener("keydown", function(event) {
      if (event.key === "ArrowUp" && dy !== 10) {
        dx = 0; dy = -10;
      }
      else if (event.key === "ArrowDown" && dy !== -10) {
        dx = 0; dy = 10;
      }
      else if (event.key === "ArrowLeft" && dx !== 10) {
        dx = -10; dy = 0;
      }
      else if (event.key === "ArrowRight" && dx !== -10) {
        dx = 10; dy = 0;
      }
    });

    startGame();
  </script>
</body>
</html>
