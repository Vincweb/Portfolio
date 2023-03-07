<template>
  <section class="gameOfLife">
   <header class="title">
      <h1 class="reveal-1">GAME OF LIFE</h1>
      <h3 class="reveal-2">John Horton Conway</h3>
    </header>
    <article class="board">
      <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
        <div
          v-for="(cell, colIndex) in row"
          :key="colIndex"
          class="cell"
          :class="{ alive: cell === 1 }"
        />
      </div>
    </article>
    </section>
</template>

<script>
export default {
  data() {
    return {
      rows: 40,
      cols: 100,
      board: [],
      interval: null
    };
  },
  created() {
    this.initializeBoard();
  },
  mounted() {
    const observerGame = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if(entry.intersectionRatio > 0) {
          this.startGame();
        } else {
          this.stopGame();
        }
      })
    })
    observerGame.observe(document.querySelector('.board'));

    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if(entry.intersectionRatio > 0) {
          entry.target.classList.add('reveal-visible');
          observer.unobserve(entry.target);
        }
      })
    })
    document.querySelectorAll('[class*="reveal-"]').forEach((r) => {
      observer.observe(r);
    })
  },
  methods: {
    initializeBoard() {
      const board = [];

      for (let i = 0; i < this.rows; i++) {
        const row = [];
        for (let j = 0; j < this.cols; j++) {
          row.push(Math.floor(Math.random() * 2));
        }
        board.push(row);
      }

      this.board = board;
    },
    startGame() {
      this.interval = setInterval(() => {
        this.evolve();
      }, 1000);
    },
    stopGame() {
      clearInterval(this.interval);
    },
    evolve() {
      const newBoard = JSON.parse(JSON.stringify(this.board));

      for (let i = 0; i < this.rows; i++) {
        for (let j = 0; j < this.cols; j++) {
          const cell = this.board[i][j];
          let neighbors = 0;

          for (let x = -1; x <= 1; x++) {
            for (let y = -1; y <= 1; y++) {
              if (x === 0 && y === 0) {
                continue;
              }

              const row = (i + x + this.rows) % this.rows;
              const col = (j + y + this.cols) % this.cols;

              neighbors += this.board[row][col];
            }
          }

          if (cell === 1) {
            if (neighbors < 2 || neighbors > 3) {
              newBoard[i][j] = 0;
            }
          } else {
            if (neighbors === 3) {
              newBoard[i][j] = 1;
            }
          }
        }
      }

      this.board = newBoard;
    }
  }
};
</script>

<style scoped>
.gameOfLife{
  position:relative;
}
.board {
  background-color: black;
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  overflow: hidden;
}
.row {
  display: flex;
}
.cell {
  width: 20px;
  height: 20px;
  border: 1px solid black;
  background-color: black;
}
.alive {
  background-color: white;
  /* transition: .5s; */
}
.title{
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  color: white;
  text-align: right;
}
.title h1{
  font-size: 5rem;
  padding-right: 20px;
  text-shadow: 2px 2px 4px #000000;
}
.title h3{
  padding-right: 20px;
  text-shadow: 2px 2px 4px #000000;
}
[class*="reveal-"]{
  opacity: 0;
  transform: translateY(-30px)
}
.reveal-visible{
  opacity: 1;
  transform: translateY(0);
  transition: 2s cubic-bezier(.5, 0, 0, 1);
}
.reveal-2{
  transition-delay: .1s;
}
</style>
