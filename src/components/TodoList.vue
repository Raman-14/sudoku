<template>
  <div class="sudoku-container">
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
    </div>
    <button class="check-button" @click="checkSudoku">Check Sudoku</button>
    <div v-if="isSudokuValid !== null">
      <p class="result">{{ isSudokuValid ? "Sudoku is valid!" : "Sudoku is invalid!" }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      grid: [
        [5, 3, ""],
        [6, "", ""],
        ["", 9, 8]
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
      for (let row = 0; row < 3; row++) {
        const st = new Set();
        for (let col = 0; col < 3; col++) {
          if (this.grid[row][col] === "") continue;
          if (st.has(this.grid[row][col])) {
            this.isSudokuValid = false;
            return;
          }
          st.add(this.grid[row][col]);
        }
      }

      // Check each column
      for (let col = 0; col < 3; col++) {
        const st = new Set();
        for (let row = 0; row < 3; row++) {
          if (this.grid[row][col] === "") continue;
          if (st.has(this.grid[row][col])) {
            this.isSudokuValid = false;
            return;
          }
          st.add(this.grid[row][col]);
        }
      }

      // Check the 3x3 subgrid
      const st = new Set();
      for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
          if (this.grid[row][col] === "") continue;
          if (st.has(this.grid[row][col])) {
            this.isSudokuValid = false;
            return;
          }
          st.add(this.grid[row][col]);
        }
      }

      this.isSudokuValid = true;
    }
  }
}
</script>

<style>
.sudoku-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
}

.sudoku-grid {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 120px; /* Adjusted for 3x3 grid */
  margin: auto;
}

.row {
  display: flex;
}

input {
  width: 100%;
  height: 40px;
  text-align: center;
  margin: 1px;
  font-size: 1.5em;
  box-sizing: border-box;
}

.check-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1.2em;
}

.result {
  margin-top: 10px;
  font-size: 1.2em;
}

@media (max-width: 600px) {
  .sudoku-grid {
    max-width: 100%;
  }

  input {
    height: 30px;
    font-size: 1.2em;
  }

  .check-button {
    font-size: 1em;
  }

  .result {
    font-size: 1em;
  }
}
</style>
