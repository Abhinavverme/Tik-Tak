<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic Tac Toe</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: radial-gradient(#1a0033, #000);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    #start-screen, #game-screen {
      text-align: center;
      width: 100%;
      padding: 20px;
    }

    #start-screen button {
      font-size: 24px;
      padding: 15px 40px;
      border: none;
      border-radius: 10px;
      background: #00f0ff;
      color: #000;
      cursor: pointer;
      box-shadow: 0 0 15px #00f0ff;
    }

    #game-screen {
      display: none;
    }

    .status {
      font-size: 20px;
      margin-bottom: 10px;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      max-width: 300px;
      margin: 20px auto;
    }

    .cell {
      width: 100px;
      height: 100px;
      background: transparent;
      border: 2px solid #888;
      font-size: 48px;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 15px;
      box-shadow: 0 0 10px #444;
      cursor: pointer;
      transition: 0.2s;
    }

    .cell.glowO {
      color: #00f0ff;
      text-shadow: 0 0 10px #00f0ff, 0 0 20px #00f0ff;
    }

    .cell.glowX {
      color: #ff6600;
      text-shadow: 0 0 10px #ff6600, 0 0 20px #ff6600;
    }

    .winner-line {
      position: absolute;
      width: 100%;
      height: 4px;
      background: #00f0ff;
      transform-origin: left;
    }

    @media (max-width: 500px) {
      .cell {
        width: 80px;
        height: 80px;
        font-size: 40px;
      }
    }
  </style>
</head>
<body>
  <div id="start-screen">
    <h1>Tic Tac Toe</h1>
    <button onclick="startGame()">Start Game</button>
  </div>

  <div id="game-screen">
    <h2><span style="color:#00f0ff">O</span> vs <span style="color:#ff6600">X</span></h2>
    <div class="status" id="status">Your Turn</div>
    <div class="board" id="board"></div>
    <div id="result" style="font-size: 20px; margin-top: 20px;"></div>
  </div>

  <script>
    const startScreen = document.getElementById("start-screen");
    const gameScreen = document.getElementById("game-screen");
    const board = document.getElementById("board");
    const statusText = document.getElementById("status");
    const resultText = document.getElementById("result");

    let cells = [];
    let currentPlayer = "O";
    let gameActive = true;

    function startGame() {
      startScreen.style.display = "none";
      gameScreen.style.display = "block";
      createBoard();
    }

    function createBoard() {
      board.innerHTML = "";
      cells = Array(9).fill(null);
      for (let i = 0; i < 9; i++) {
        const cell = document.createElement("div");
        cell.classList.add("cell");
        cell.dataset.index = i;
        cell.addEventListener("click", () => handleMove(i));
        board.appendChild(cell);
      }
    }

    function handleMove(index) {
      if (!gameActive || cells[index]) return;

      cells[index] = currentPlayer;
      const cellDiv = board.children[index];
      cellDiv.textContent = currentPlayer;
      cellDiv.classList.add(currentPlayer === "O" ? "glowO" : "glowX");

      if (checkWin(currentPlayer)) {
        resultText.innerHTML = `<span style="color:#00f0ff">${currentPlayer}</span> Wins!`;
        gameActive = false;
        return;
      }

      if (!cells.includes(null)) {
        resultText.textContent = "It's a Draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === "O" ? "X" : "O";
      statusText.textContent = currentPlayer === "O" ? "Your Turn" : "Computer Thinking...";
      
      if (currentPlayer === "X") {
        setTimeout(computerMove, 600);
      }
    }

    function computerMove() {
      if (!gameActive) return;
      let emptyIndexes = cells.map((v, i) => (v === null ? i : null)).filter(v => v !== null);
      let move = emptyIndexes[Math.floor(Math.random() * emptyIndexes.length)];
      handleMove(move);
    }

    function checkWin(player) {
      const winConditions = [
        [0,1,2],[3,4,5],[6,7,8],
        [0,3,6],[1,4,7],[2,5,8],
        [0,4,8],[2,4,6]
      ];
      return winConditions.some(combo =>
        combo.every(i => cells[i] === player)
      );
    }
  </script>
</body>
</html>
