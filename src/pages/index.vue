<script setup lang="ts">
import { makepuzzle, ratepuzzle, solvepuzzle } from 'sudoku'
import { useFocus } from '@vueuse/core'

const board: any = reactive({})

const puzzle = makepuzzle()
const rightAnswer = solvepuzzle(puzzle).map((i: number) => i + 1)

const colorPlatte = {
  1: 'color-blue-500',
  2: 'color-rose-400',
  3: 'color-violet-400',
  4: 'color-indigo-400',
  5: 'color-cyan-400',
  6: 'color-green-400',
  7: 'color-yellow-400',
  8: 'color-black',
  9: 'color-red-500',
}
const answer = [...puzzle].map((i: number) => i + 1)
function initBoard(size: number) {
  for (let i = 0; i < size; i++) {
    board[i] = []
    for (let j = 0; j < size; j++) {
      board[i].push({
        col: i,
        row: j,
        number: puzzle[i * 9 + j] !== null ? puzzle[i * 9 + j] + 1 : null,
      })
    }
  }
}

function handleInput(row: number, col: number) {
  answer[col * 9 + row] = Number(board[col][row].value)
  // const { focused: inputFocus } = useFocus(board[col][row].value)
  // inputFocus.value = false
}

const showResult = ref(false)
function checkAnswer() {
  console.log(answer)
  console.log(rightAnswer)
  if (answer === rightAnswer)
    alert('You are right!')

  else
    alert('You are wrong!')
}

onMounted(() => {
  initBoard(9)
})
</script>

<template>
  <div>
    <h2 text-2xl>
      Sudoku
    </h2>
    <div>in Dev Program 🦄</div>
    <div py-4 />
    <div flex flex-col items-center justify-center>
      <div v-for="(row, idx) in board" :key="idx" flex>
        <div
          v-for="(grid, gridIdx) in row" :key="gridIdx"
          border text-center w-7 h-7 flex items-center justify-center
          hover:bg-gray-200 bg:op-70
        >
          <div v-if="grid.number">
            <div :class="colorPlatte[grid.number]" font-bold>
              {{ grid.number }}
            </div>
          </div>
          <input
            v-else v-model="board[grid.col][grid.row].value" type="number"
            w-full h-full text-center border-gray-300
            @keyup.enter="handleInput(grid.row, grid.col)"
          >
        </div>
      </div>
    </div>
  </div>
  <div pt-4>
    <button
      border px-2 rounded-sm hover:bg-gray-100
      @click="checkAnswer"
    >
      Check
    </button>
  </div>
</template>

<style>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
}
input[type="number"]{
    -moz-appearance: textfield;
}
</style>
