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
  <div>
    <h1>Random Quote</h1>
    <button @click="fetchQuote">Get New Quote</button>
    <p v-if="quote">{{ quote }}</p>
    <p v-else>Click the button to get a quote.</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// State for adding numbers
const num1 = ref(0)
const num2 = ref(0)
const result = ref(null)

// State for fetching quote
const quote = ref('')

// Function to handle adding two numbers
const handleSubmit = async () => {
  try {
    const response = await $fetch(`https://mohiththarun.azurewebsites.net/api/Math/${num1.value}/${num2.value}`)
    result.value = response
  } catch (error) {
    console.error('Error:', error)
  }
}

// Function to fetch the random quote from the API
const fetchQuote = async () => {
  try {
    const response = await $fetch('http://localhost:5004/api/quotes')
    quote.value = response.quote
  } catch (error) {
    console.error('Error fetching quote:', error)
  }
}

// Fetch a quote when the component is mounted
onMounted(() => {
  fetchQuote()
})
</script>
