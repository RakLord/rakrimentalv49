<template>
  <div>
    <h1>Incremental Game</h1>
    <p>Currency: {{ currency.toFixed(2) }}</p>
    <button @click="manualClick">Click Me!</button>
    <button @click="saveGame">Save</button>
    <button @click="loadGame">Load</button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Game State
const currency = ref(0)
const currencyPerSecond = ref(1)

// Game Loop
let gameLoopInterval = null

const gameLoop = () => {
  currency.value += currencyPerSecond.value / 10 // Update currency every 100ms
}

onMounted(() => {
  loadGame()
  gameLoopInterval = setInterval(gameLoop, 100)
})

onUnmounted(() => {
  clearInterval(gameLoopInterval)
})

// Manual Click
const manualClick = () => {
  currency.value++
}

// Save/Load System
const saveGame = () => {
  const gameState = {
    currency: currency.value,
    currencyPerSecond: currencyPerSecond.value,
  }
  localStorage.setItem('incremental_game_save', JSON.stringify(gameState))
  alert('Game Saved!')
}

const loadGame = () => {
  const savedState = localStorage.getItem('incremental_game_save')
  if (savedState) {
    const gameState = JSON.parse(savedState)
    currency.value = gameState.currency
    currencyPerSecond.value = gameState.currencyPerSecond
    alert('Game Loaded!')
  }
}
</script>

<style scoped>
p {
  font-size: 1.5em;
}

button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 1em;
  cursor: pointer;
}
</style>
