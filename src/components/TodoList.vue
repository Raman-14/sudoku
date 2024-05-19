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
       v-bind:readonly="grid[rowIndex][colIndex] !== ''"
       />
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
    };
  },
  methods: {
  validateInput(event, rowIndex, colIndex) {
    const value = event.target.value;
    const number = parseInt(value);
   

    // Check if the value is a single character and between 1 and 9
    if (value.length === 1 && number >= 1 && number <= 9) {
      this.grid[rowIndex][colIndex] = number;
    } else if (value === "") {
      this.grid[rowIndex][colIndex] = "";
    } else {
      event.target.value = "";
      this.grid[rowIndex][colIndex] = "";
    }
  }
}
}
</script>

<style >
.sudoku-grid{
  display:flex;
  flex-direction:column;
  width:180px;
  margin:auto;
}
.row{
  display:flex;
}
input{
  width:20px;
  height:20px;
  text-align:center;
  margin:2px;
}

</style>
