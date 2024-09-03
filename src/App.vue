<template>
  <div
    class="gameboard w-96 h-96 mx-auto mt-36 flex flex-col"
  >
    <div
      v-for="(row, rowIndex) in gameboard"
      :key="rowIndex"
      class="gameboard__row  border-amber-400 flex-1 flex"
    >
      <div
        v-for="(cell, cellIndex) in row"
        :key="cellIndex"
        class="gameboard__cell border-2 flex-1 text-white flex justify-center items-center text-8xl relative"
        @click="play(rowIndex,cellIndex)"
      >
        {{ cell === 1 ? 'X' : cell === -1 ? 'O' : '' }}
      </div>
    </div>
  </div>

  <div v-if="winner">
    <button
      class="text-white flex mx-auto border-2 p-4 rounded-lg mt-3 hover:bg-blue-600"
      @click="restart"
    >
      Start New Game
    </button>

    <div>
      <h1
        class="text-white text-6xl text-center mt-6"
      >
        Winner Is : {{ winner }}
      </h1>
    </div>
  </div>
</template>
<script setup>

import {computed, ref} from 'vue'

const player = ref(1)
const winner = ref('')
const gameboard = ref([
    [0,0,0],
    [0,0,0],
    [0,0,0],
])

const restart = () => {
  player.value = 1
  winner.value = ''
  gameboard.value = [
    [0,0,0], // si total cumulé égal 3 victoire X
    [0,0,0], // si total cumulé égal -3 victoire 0
    [0,0,0],
  ]
}

const switchPlayer = (playerValue) => {
  return playerValue === 1 ? player.value= -1 : player.value = 1
}

function play(rowIndex, cellIndex) {
  if(gameboard.value[rowIndex][cellIndex] === 0 && winner.value === '') {
    gameboard.value[rowIndex][cellIndex] = player.value
    switchPlayer(player.value)
    calculateVictory(rowIndex)
  }
}

function calculateVictory (rowIndex) {
  // Check if it's a draw
  const draw = gameboard.value.flat().every(cell => cell !== 0)


  // Check victory condition in diagonals
  const firstDiag = computed(() => gameboard.value[0][0] + gameboard.value[1][1] + gameboard.value[2][2])
  const secondDiag = computed(() => gameboard.value[0][2] + gameboard.value[1][1] + gameboard.value[2][0])

  // Check victory condition in rows
  const rowTotalValue = computed(() => gameboard.value[rowIndex].reduce((acc,currentValue) => acc + currentValue, 0))

  // Check victory condition in  columns
  const firstCol = computed(() => gameboard.value[0][0] + gameboard.value[1][0] + gameboard.value[2][0])
  const secondCol = computed(() => gameboard.value[0][1] + gameboard.value[1][1] + gameboard.value[2][1])
  const thirdCol = computed(() => gameboard.value[0][2] + gameboard.value[1][2] + gameboard.value[2][2])

  // Condition de victoire
  if (rowTotalValue.value === 3 || firstCol.value === 3 || secondCol.value === 3 || thirdCol.value === 3 || firstDiag.value === 3 || secondDiag.value === 3) {
    winner.value = 'X'
  } else if (rowTotalValue.value === -3 || firstCol.value === -3 || secondCol.value === -3 || thirdCol.value === -3 || firstDiag.value === -3 || secondDiag.value === -3) {
    winner.value = 'O'
  } else if (draw) {
    winner.value = 'Draw'
  }
}

</script>
