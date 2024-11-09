<!DOCTYPE html>
<html>
<head>
  <title>Basic Snake HTML Game</title>
  <meta charset="UTF-8">
  <style>
    html, body {
      height: 100%;
      margin: 0;
    }
    body {
      background: black;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    canvas {
      border: 1px solid white;
      margin-bottom: 10px;
    }
    .buttons {
      display: grid;
      grid-template-columns: repeat(3, 60px);
      grid-gap: 10px;
      justify-content: center;
    }
    button {
      padding: 10px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <canvas width="400" height="400" id="game"></canvas>
  <div class="buttons">
    <br> <button onclick="moveLeft()">Left</button>
    </br> 
    <button onclick="moveUp()">Up</button>
    <br><button onclick="moveRight()">Right</button></br>
    
    <button onclick="moveDown()">Down</button>
  </div>

  <script>
    var canvas = document.getElementById('game');
    var context = canvas.getContext('2d');
    var grid = 16;
    var count = 0;

    var snake = {
      x: 160,
      y: 160,
      dx: grid,
      dy: 0,
      cells: [],
      maxCells: 4
    };
    var apple = {
      x: 320,
      y: 320
    };

    function getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    }

    function loop() {
      requestAnimationFrame(loop);

      if (++count < 4) {
        return;
      }

      count = 0;
      context.clearRect(0, 0, canvas.width, canvas.height);

      snake.x += snake.dx;
      snake.y += snake.dy;

      if (snake.x < 0) snake.x = canvas.width - grid;
      else if (snake.x >= canvas.width) snake.x = 0;

      if (snake.y < 0) snake.y = canvas.height - grid;
      else if (snake.y >= canvas.height) snake.y = 0;

      snake.cells.unshift({ x: snake.x, y: snake.y });

      if (snake.cells.length > snake.maxCells) {
        snake.cells.pop();
      }

      context.fillStyle = 'red';
      context.fillRect(apple.x, apple.y, grid - 1, grid - 1);

      context.fillStyle = 'green';
      snake.cells.forEach(function(cell, index) {
        context.fillRect(cell.x, cell.y, grid - 1, grid - 1);

        if (cell.x === apple.x && cell.y === apple.y) {
          snake.maxCells++;
          apple.x = getRandomInt(0, 25) * grid;
          apple.y = getRandomInt(0, 25) * grid;
        }

        for (var i = index + 1; i < snake.cells.length; i++) {
          if (cell.x === snake.cells[i].x && cell.y === snake.cells[i].y) {
            snake.x = 160;
            snake.y = 160;
            snake.cells = [];
            snake.maxCells = 4;
            snake.dx = grid;
            snake.dy = 0;

            apple.x = getRandomInt(0, 25) * grid;
            apple.y = getRandomInt(0, 25) * grid;
          }
        }
      });
    }

    // Movement functions for buttons
    function moveLeft() {
      if (snake.dx === 0) {
        snake.dx = -grid;
        snake.dy = 0;
      }
    }

    function moveUp() {
      if (snake.dy === 0) {
        snake.dy = -grid;
        snake.dx = 0;
      }
    }

    function moveRight() {
      if (snake.dx === 0) {
        snake.dx = grid;
        snake.dy = 0;
      }
    }

    function moveDown() {
      if (snake.dy === 0) {
        snake.dy = grid;
        snake.dx = 0;
      }
    }

    // Keyboard controls for additional support
    document.addEventListener('keydown', function(e) {
      if (e.which === 37) moveLeft();
      else if (e.which === 48) moveUp();
      else if (e.which === 59) moveRight();
      else if (e.which === 40) moveDown();
    });

    requestAnimationFrame(loop);
  </script>
</body>
</html>