# game.jefan<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tap Kotak</title>
  <style>
    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(180deg, #0f172a, #1e293b);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 16px;
    }

    .container {
      width: 100%;
      max-width: 480px;
      text-align: center;
    }

    h1 {
      margin-bottom: 8px;
      font-size: 30px;
    }

    p {
      margin-top: 0;
      color: #cbd5e1;
    }

    .info {
      display: flex;
      justify-content: space-between;
      gap: 10px;
      margin: 16px 0;
      font-size: 18px;
      background: rgba(255,255,255,0.08);
      padding: 12px 16px;
      border-radius: 12px;
    }

    button {
      background: #22c55e;
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 12px;
      font-size: 18px;
      cursor: pointer;
      margin-bottom: 16px;
      width: 100%;
      font-weight: bold;
    }

    button:active {
      transform: scale(0.98);
    }

    #gameArea {
      position: relative;
      width: 100%;
      height: 60vh;
      min-height: 340px;
      max-height: 520px;
      background: #e2e8f0;
      border-radius: 20px;
      overflow: hidden;
      border: 4px solid white;
    }

    #target {
      position: absolute;
      width: 64px;
      height: 64px;
      background: linear-gradient(135deg, #ef4444, #b91c1c);
      border-radius: 16px;
      display: none;
      box-shadow: 0 8px 20px rgba(0,0,0,0.25);
      transition: transform 0.08s;
      touch-action: manipulation;
    }

    #message {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: #0f172a;
      font-size: 22px;
      font-weight: bold;
      text-align: center;
      width: 90%;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Tap Kotak</h1>
    <p>Sentuh kotak merah sebanyak mungkin dalam 30 detik.</p>

    <div class="info">
      <div>Skor: <b id="score">0</b></div>
      <div>Waktu: <b id="time">30</b></div>
    </div>

    <button id="startBtn">Mulai / Ulangi Game</button>

    <div id="gameArea">
      <div id="target"></div>
      <div id="message">Tekan tombol "Mulai" untuk bermain</div>
    </div>
  </div>

  <script>
    const gameArea = document.getElementById("gameArea");
    const target = document.getElementById("target");
    const scoreEl = document.getElementById("score");
    const timeEl = document.getElementById("time");
    const startBtn = document.getElementById("startBtn");
    const message = document.getElementById("message");

    let score = 0;
    let timeLeft = 30;
    let gameTimer = null;
    let moveTimer = null;
    let playing = false;
    const targetSize = 64;

    function randomPosition() {
      const rect = gameArea.getBoundingClientRect();
      const maxX = rect.width - targetSize;
      const maxY = rect.height - targetSize;

      const x = Math.random() * maxX;
      const y = Math.random() * maxY;

      target.style.left = x + "px";
      target.style.top = y + "px";
    }

    function startGame() {
      clearInterval(gameTimer);
      clearInterval(moveTimer);

      score = 0;
      timeLeft = 30;
      playing = true;

      scoreEl.textContent = score;
      timeEl.textContent = timeLeft;
      message.textContent = "";
      target.style.display = "block";

      randomPosition();

      moveTimer = setInterval(() => {
        if (playing) randomPosition();
      }, 800);

      gameTimer = setInterval(() => {
        timeLeft--;
        timeEl.textContent = timeLeft;

        if (timeLeft <= 0) {
          endGame();
        }
      }, 1000);
    }

    function endGame() {
      playing = false;
      clearInterval(gameTimer);
      clearInterval(moveTimer);
      target.style.display = "none";
      message.textContent = "Waktu habis! Skor kamu: " + score;
    }

    function hitTarget() {
      if (!playing) return;

      score++;
      scoreEl.textContent = score;

      target.style.transform = "scale(0.9)";
      setTimeout(() => {
        target.style.transform = "scale(1)";
      }, 80);

      randomPosition();
    }

    startBtn.addEventListener("click", startGame);
    target.addEventListener("click", hitTarget);
    target.addEventListener("touchstart", function(e) {
      e.preventDefault();
      hitTarget();
    }, { passive: false });
  </script>
</body>
</html>
