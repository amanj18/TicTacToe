<template>
  <div id="app">
    <div class="game-container">
      <h1 class="game-title">Tic Tac Toe </h1>
      <div class="board">
        <div v-for="(cell, index) in board" :key="index" class="cell" :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O', 'winning-cell': winningCells.includes(index) }" @click="makeMove(index)">
          <span class="cell-content">{{ cell }}</span>
        </div>
      </div>
      <div v-if="winner" class="winner">
        <h2>Winner: {{ winner }}</h2>
        <button @click="resetGame" class="play-again-btn">Play Again</button>
      </div>
      <div v-else-if="isBoardFull" class="winner">
        <h2>It's a Draw!</h2>
        <button @click="resetGame" class="play-again-btn">Play Again</button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: 'X',
      winner: null,
      winningCells: [],
    };
  },
  computed: {
    isBoardFull() {
      return this.board.every(cell => cell !== null);
    }
  },
  methods: {
    makeMove(index) {
      if (!this.board[index] && !this.winner) {
        this.$set(this.board, index, this.currentPlayer);
        if (this.checkWinner()) {
          this.winner = this.currentPlayer;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      }
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ];

      for (let combination of winningCombinations) {
        const [a, b, c] = combination;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          this.winningCells = combination;
          return true;
        }
      }
      return false;
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
      this.winningCells = [];
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');

:root {
  --color-bg-1: #ff6b6b;
  --color-bg-2: #4ecdc4;
  --color-bg-3: #feca57;
  --color-bg-4: #6a11cb;
  --color-o: #42c3c3;
  --color-title: #ffffff;
  --color-cell: rgba(230, 217, 70);
  --color-btn: #ffd166;
  --color-btn-hover: #ffc233;
  --color-winning-line: #4caf50;
}


body {
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(-45deg, var(--color-bg-1), var(--color-bg-2), var(--color-bg-3), var(--color-bg-4));
  background-size: 400% 400%;
  animation: gradientBG 15s ease infinite;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}


#app {
  text-align: center;
  position: relative;
}

.game-container {
  background-color: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  animation: containerPulse 4s ease-in-out infinite;
}


.game-title {
  font-size: 3.5rem;
  color: var(--color-title);
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  animation: titleGlow 2s ease-in-out infinite alternate;
}



.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
  justify-content: center;
  margin: 20px auto;
}

.cell {
  background-color:#69e8c0;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5rem;
  font-weight: bold;
  cursor: pointer;
  border-radius: 15px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.cell::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, rgba(255,255,255,0) 70%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.cell:hover::before {
  opacity: 1;
  animation: cellShine 1s forwards;
}


.cell:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
  background-color: #6fe4f1;
}

.cell-x {
  color:rgb(199, 109, 0);
  text-shadow: 0 0 10px var(--color-x);
}

.cell-o {
  color: rgb(197, 0, 131);
  text-shadow: 0 0 10px var(--color-o);
}

.winning-cell {
  animation: winningCellPulse 1s ease-in-out infinite alternate;
}



.winning-cell::after {
  content: '';
  position: absolute;
  color: #4caf50;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--color-winning-line);
  opacity: 0.3;
  animation: winningCellFill 0.5s ease-out forwards;
}


.winner {
  margin-top: 30px;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(5px);
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  animation: winnerAppear 0.5s ease-out;
}

.play-again-btn {
  background-color: var(--color-btn);
  color: #333;
  border: none;
  padding: 10px 20px;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
}

.play-again-btn:hover {
  background-color: var(--color-btn-hover);
  transform: translateY(-2px);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
}

.cell-content {
  animation: fadeIn 0.3s ease-out;
}


.game-container::after {
  width: 15px;
  height: 15px;
  animation-duration: 15s;
  animation-delay: -5s;
}

</style>