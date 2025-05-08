<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mobile Tetris</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: #111;
      display: flex;
      flex-direction: column;
      align-items: center;
      color: white;
    }
    canvas {
      background: #222;
      margin-top: 20px;
    }
    .controls {
      display: flex;
      margin-top: 10px;
      gap: 10px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      background: #555;
      color: white;
    }
  </style>
</head>
<body>
  <h1>Tetris Mobile</h1>
  <canvas id="tetris" width="240" height="400"></canvas>
  <div class="controls">
    <button onclick="move(-1)">Left</button>
    <button onclick="rotate()">Rotate</button>
    <button onclick="move(1)">Right</button>
    <button onclick="drop()">Down</button>
  </div>  <script>
    const canvas = document.getElementById('tetris');
    const context = canvas.getContext('2d');
    context.scale(20, 20);

    const arena = createMatrix(12, 20);
    const player = {
      pos: {x: 5, y: 0},
      matrix: createPiece('T'),
    };

    function createMatrix(w, h) {
      const matrix = [];
      while (h--) matrix.push(new Array(w).fill(0));
      return matrix;
    }

    function createPiece(type) {
      if (type === 'T') {
        return [
          [0, 1, 0],
          [1, 1, 1],
          [0, 0, 0],
        ];
      }
      // You can add more pieces later
    }

    function drawMatrix(matrix, offset) {
      matrix.forEach((row, y) => {
        row.forEach((value, x) => {
          if (value) {
            context.fillStyle = 'purple';
            context.fillRect(x + offset.x, y + offset.y, 1, 1);
          }
        });
      });
    }

    function draw() {
      context.fillStyle = '#000';
      context.fillRect(0, 0, canvas.width, canvas.height);

      drawMatrix(player.matrix, player.pos);
    }

    function merge(arena, player) {
      player.matrix.forEach((row, y) => {
        row.forEach((value, x) => {
          if (value) arena[y + player.pos.y][x + player.pos.x] = value;
        });
      });
    }

    function collide(arena, player) {
      const [m, o] = [player.matrix, player.pos];
      for (let y = 0; y < m.length; y++) {
        for (let x = 0; x < m[y].length; x++) {
          if (m[y][x] && (arena[y + o.y] && arena[y + o.y][x + o.x]) !== 0) {
            return true;
          }
        }
      }
      return false;
    }

    function playerDrop() {
      player.pos.y++;
      if (collide(arena, player)) {
        player.pos.y--;
        merge(arena, player);
        playerReset();
      }
      dropCounter = 0;
    }

    function playerMove(dir) {
      player.pos.x += dir;
      if (collide(arena, player)) player.pos.x -= dir;
    }

    function playerRotate() {
      const m = player.matrix;
      for (let y = 0; y < m.length; ++y) {
        for (let x = 0; x < y; ++x) {
          [m[x][y], m[y][x]] = [m[y][x], m[x][y]];
        }
      }
      m.forEach(row => row.reverse());
      if (collide(arena, player)) player.matrix.reverse();
    }

    function playerReset() {
      player.matrix = createPiece('T');
      player.pos.y = 0;
      player.pos.x = (arena[0].length / 2 | 0) - (player.matrix[0].length / 2 | 0);
      if (collide(arena, player)) {
        arena.forEach(row => row.fill(0));
        alert('Game Over');
      }
    }

    let dropCounter = 0;
    let dropInterval = 1000;
    let lastTime = 0;

    function update(time = 0) {
      const deltaTime = time - lastTime;
      lastTime = time;
      dropCounter += deltaTime;
      if (dropCounter > dropInterval) playerDrop();
      draw();
      requestAnimationFrame(update);
    }

    function move(dir) { playerMove(dir); }
    function rotate() { playerRotate(); }
    function drop() { playerDrop(); }

    playerReset();
    update();
  </script></body>
</html>
