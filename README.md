# SCT_WD_3
TIC-TAC-TOE WEB APPLICATION
let board = ["", "", "", "", "", "", "", "", ""];
let currentPlayer = "X";
let gameRunning = true;
let gameMode = "pvp";

const cells = document.querySelectorAll(".cell");
const statusText = document.getElementById("status");

const winPatterns = [
  [0,1,2], [3,4,5], [6,7,8],
  [0,3,6], [1,4,7], [2,5,8],
  [0,4,8], [2,4,6]
];

statusText.textContent = "Player X's turn";

cells.forEach((cell, index) => {
  cell.addEventListener("click", () => handleMove(index));
});

function setMode(mode) {
  gameMode = mode;
  resetGame();
}

function handleMove(index) {
  if (board[index] !== "" || !gameRunning) return;

  board[index] = currentPlayer;
  cells[index].textContent = currentPlayer;

  if (checkWin()) {
    statusText.textContent = `Player ${currentPlayer} wins`;
    gameRunning = false;
    return;
  }

  if (!board.includes("")) {
    statusText.textContent = "Draw game";
    gameRunning = false;
    return;
  }

  currentPlayer = currentPlayer === "X" ? "O" : "X";
  statusText.textContent = `Player ${currentPlayer}'s turn`;

  if (gameMode === "cpu" && currentPlayer === "O") {
    setTimeout(computerMove, 500);
  }
}

function computerMove() {
  const emptyCells = board
    .map((value, index) => value === "" ? index : null)
    .filter(value => value !== null);

  const randomMove = emptyCells[Math.floor(Math.random() * emptyCells.length)];
  handleMove(randomMove);
}

function checkWin() {
  return winPatterns.some(pattern =>
    pattern.every(index => board[index] === currentPlayer)
  );
}

function resetGame() {
  board = ["", "", "", "", "", "", "", "", ""];
  cells.forEach(cell => cell.textContent = "");
  currentPlayer = "X";
  gameRunning = true;
  statusText.textContent = "Player X's turn";
}
