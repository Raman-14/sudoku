<template>
  <div class="sudoku-grid">
    <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="row">
      <input
        v-for="(col, colIndex) in row"
        :key="colIndex"
        class="col"
        type="text"
        maxlength="1"
        v-model="grid[rowIndex][colIndex]"
        v-on:input="validateInput($event, rowIndex, colIndex)"
      />
    </div>
    <button @click="checkSudoku">Check Sudoku</button>
    <div v-if="isSudokuValid !== null">
      {{ isSudokuValid ? "Sudoku is valid!" : "Sudoku is invalid!" }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      grid: [
        [5, 3, "", "", 7, "", "", "", ""],
        [6, "", "", 1, 9, 5, "", "", ""],
        ["", 9, 8, "", "", "", "", 6, ""],
        [8, "", "", "", 6, "", "", "", 3],
        [4, "", "", 8, "", 3, "", "", 1],
        [7, "", "", "", 2, "", "", "", 6],
        ["", 6, "", "", "", "", 2, 8, ""],
        ["", "", "", 4, 1, 9, "", "", 5],
        ["", "", "", "", 8, "", "", 7, 9],
      ],
      isSudokuValid: null,
    };
  },
  methods: {
    validateInput(event, rowIndex, colIndex) {
      const value = event.target.value;
      const number = parseInt(value);

      if (value.length === 1 && number >= 1 && number <= 9) {
        this.grid[rowIndex][colIndex] = number;
      } else if (value === "") {
        this.grid[rowIndex][colIndex] = "";
      } else {
        event.target.value = "";
        this.grid[rowIndex][colIndex] = "";
      }
    },
    checkSudoku() {
      // Check each row
      for (let row = 0; row < 9; row++) {
        const st = new Set();
        for (let col = 0; col < 9; col++) {
          if (this.grid[row][col] === "") continue;
          if (st.has(this.grid[row][col])) {
            this.isSudokuValid = false;
            return;
          }
          st.add(this.grid[row][col]);
        }
      }

      // Check each column
      for (let col = 0; col < 9; col++) {
        const st = new Set();
        for (let row = 0; row < 9; row++) {
          if (this.grid[row][col] === "") continue;
          if (st.has(this.grid[row][col])) {
            this.isSudokuValid = false;
            return;
          }
          st.add(this.grid[row][col]);
        }
      }

      // Check each 3x3 subgrid
      for (let sr = 0; sr < 9; sr += 3) {
        for (let sc = 0; sc < 9; sc += 3) {
          if (!this.validBox(this.grid, sr, sr + 2, sc, sc + 2)) {
            this.isSudokuValid = false;
            return;
          }
        }
      }

      this.isSudokuValid = true;
    },
    validBox(board, sr, er, sc, ec) {
      const st = new Set();
      for (let i = sr; i <= er; i++) {
        for (let j = sc; j <= ec; j++) {
          if (board[i][j] === "") continue;
          if (st.has(board[i][j])) return false;
          st.add(board[i][j]);
        }
      }
      return true;
    }
  }
}
</script>

<style>
.sudoku-grid {
  display: flex;
  flex-direction: column;
  width: 180px;
  margin: auto;
}
.row {
  display: flex;
}
input {
  width: 20px;
  height: 20px;
  text-align: center;
  margin: 2px;
}
</style>
