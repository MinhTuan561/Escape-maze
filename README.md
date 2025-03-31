# Escape-maze<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Trốn Thoát Mê Cung</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background-color: #f0f0f0;
      overflow: hidden;
    }
    #maze {
      width: 400px;
      height: 400px;
      border: 2px solid #333;
      position: relative;
      background-color: #fff;
    }
    .player, .goal, .wall, .item, .obstacle {
      width: 20px;
      height: 20px;
      position: absolute;
    }
    .player {
      background-color: red;
      transition: top 0.2s, left 0.2s; /* Thêm hiệu ứng chuyển động mượt mà */
    }
    .goal {
      background-color: green;
    }
    .wall {
      background-color: #333;
    }
    .item {
      background-color: gold;
      border-radius: 50%;
    }
    .obstacle {
      background-color: purple;
    }
    #joystick {
      width: 100px;
      height: 100px;
      border: 2px solid #333;
      border-radius: 50%;
      position: absolute;
      bottom: 50px;
      left: 50px;
      background-color: rgba(0, 0, 0, 0.1);
      touch-action: none;
    }
    #joystickKnob {
      width: 40px;
      height: 40px;
      background-color: rgba(0, 0, 0, 0.5);
      border-radius: 50%;
      position: absolute;
      top: 30px;
      left: 30px;
      touch-action: none;
    }
    #timer, #score, #level {
      font-size: 20px;
      margin: 20px;
    }
  </style>
</head>
<body>
  <h1>Trốn Thoát Mê Cung</h1>
  <div id="maze"></div>
  <div id="joystick">
    <div id="joystickKnob"></div>
  </div>
  <div id="timer">Thời gian còn lại: <span id="time">120</span> giây</div>
  <div id="score">Điểm: <span id="points">0</span></div>
  <div id="level">Cấp độ: <span id="currentLevel">1</span></div>
  <script>
    const maze = document.getElementById('maze');
    const joystick = document.getElementById('joystick');
    const joystickKnob = document.getElementById('joystickKnob');
    const timeDisplay = document.getElementById('time');
    const scoreDisplay = document.getElementById('points');
    const levelDisplay = document.getElementById('currentLevel');
    const cellSize = 20;
    let currentLevel = 1;
    let timeLeft, points = 0, timer;

    const levels = [
      { rows: 10, cols: 10, time: 120, items: 2, obstacles: 1 },
      { rows: 15, cols: 15, time: 100, items: 4, obstacles: 2 },
      { rows: 20, cols: 20, time: 80, items: 6, obstacles: 3 }
    ];

    function generateMaze(rows, cols) {
      const mazeLayout = Array.from({ length: rows }, () =>
        Array.from({ length: cols }, () => Math.random() < 0.2 ? '1' : '0')
      );
      mazeLayout[0][0] = '0'; // Điểm bắt đầu
      mazeLayout[rows - 1][cols - 1] = '0'; // Điểm kết thúc
      return mazeLayout;
    }

    function initializeLevel() {
      const level = levels[currentLevel - 1];
      maze.innerHTML = ''; // Xóa maze hiện tại
      timeLeft = level.time;
      timeDisplay.textContent = timeLeft;
      levelDisplay.textContent = currentLevel;

      const mazeLayout = generateMaze(level.rows, level.cols);

      // Tạo tường và vật phẩm
      mazeLayout.forEach((row, rowIndex) => {
        row.forEach((cell, colIndex) => {
          if (cell === '1') {
            const wall = document.createElement('div');
            wall.classList.add('wall');
            wall.style.top = `${rowIndex * cellSize}px`;
            wall.style.left = `${colIndex * cellSize}px`;
            maze.appendChild(wall);
          } else if (Math.random() < 0.1) {
            const item = document.createElement('div');
            item.classList.add('item');
            item.style.top = `${rowIndex * cellSize}px`;
            item.style.left = `${colIndex * cellSize}px`;
            maze.appendChild(item);
          }
        });
      });

      // Thêm nhân vật và đích
      const player = document.createElement('div');
      player.classList.add('player');
      player.id = 'player';
      player.style.top = '0px';
      player.style.left = '0px';
      maze.appendChild(player);

      const goal = document.createElement('div');
      goal.classList.add('goal');
      goal.id = 'goal';
      goal.style.top = `${(level.rows - 1) * cellSize}px`;
      goal.style.left = `${(level.cols - 1) * cellSize}px`;
      maze.appendChild(goal);

      startTimer();
      spawnObstacles(level.obstacles);
    }

    function spawnObstacles(count) {
      for (let i = 0; i < count; i++) {
        const obstacle = document.createElement('div');
        obstacle.classList.add('obstacle');
        obstacle.style.top = `${Math.random() * 380}px`;
        obstacle.style.left = `${Math.random() * 380}px`;
        maze.appendChild(obstacle);

        let direction = 1;
        setInterval(() => {
          const top = parseInt(obstacle.style.top);
          const newTop = top + (10 * direction);
          if (newTop <= 0 || newTop >= maze.clientHeight - cellSize) {
            direction *= -1;
          } else {
            obstacle.style.top = `${newTop}px`;
          }
        }, 500);
      }
    }

    function startTimer() {
      clearInterval(timer);
      timer = setInterval(() => {
        timeLeft--;
        timeDisplay.textContent = timeLeft;
        if (timeLeft <= 0) {
          clearInterval(timer);
          alert('Hết thời gian! Bạn đã thua.');
        }
      }, 1000);
    }

    function checkWin() {
      const player = document.getElementById('player');
      const goal = document.getElementById('goal');
      const playerTop = parseInt(player.style.top);
      const playerLeft = parseInt(player.style.left);
      const goalTop = parseInt(goal.style.top);
      const goalLeft = parseInt(goal.style.left);

      if (playerTop === goalTop && playerLeft === goalLeft) {
        clearInterval(timer);
        if (currentLevel < levels.length) {
          alert(`Chúc mừng! Bạn đã hoàn thành cấp độ ${currentLevel}. Chuẩn bị sang cấp tiếp theo.`);
          currentLevel++;
          initializeLevel();
        } else {
          alert('Chúc mừng! Bạn đã hoàn thành tất cả các cấp độ!');
        }
      }
    }

    joystickKnob.addEventListener('touchmove', (event) => {
      const touch = event.touches[0];
      const offsetX = touch.clientX - joystick.offsetLeft - 50;
      const offsetY = touch.clientY - joystick.offsetTop - 50;

      const player = document.getElementById('player');
      const style = window.getComputedStyle(player);
      const top = parseInt(style.top);
      const left = parseInt(style.left);

      const newLeft = left + (offsetX > 30 ? cellSize : offsetX < -30 ? -cellSize : 0);
      const newTop = top + (offsetY > 30 ? cellSize : offsetY < -30 ? -cellSize : 0);

      if (newLeft >= 0 && newTop >= 0 && newLeft < 400 && newTop
