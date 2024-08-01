<template>
  <div>
    <h1>Add Two Numbers</h1>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for="num1">Number 1:</label>
        <input type="number" id="num1" v-model.number="num1" />
      </div>
      <div>
        <label for="num2">Number 2:</label>
        <input type="number" id="num2" v-model.number="num2" />
      </div>
      <button type="submit">Add</button>
    </form>
    <div v-if="result !== null">
      <h2>Result: {{ result }}</h2>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const num1 = ref(0)
const num2 = ref(0)
const result = ref(null)

const handleSubmit = async () => {
  try {
    const response = await $fetch(` https://mohiththarun.azurewebsites.net/api/Math/${num1.value}/${num2.value}`)
    result.value = response
  } catch (error) {
    console.error('Error:', error)
  }
}
</script>