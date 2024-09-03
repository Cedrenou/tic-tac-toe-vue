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
        @click="$emit('play',rowIndex, cellIndex)"
      >
        {{ cell === 1 ? 'X' : cell === -1 ? 'O' : '' }}
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['gameboard'])
defineEmits(['play'])
</script>