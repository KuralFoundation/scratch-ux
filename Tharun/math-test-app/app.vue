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

  <div>
    <h1>User Registration</h1>
    <form @submit.prevent="registerUser">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="user.name" />
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="user.email" />
      </div>
      <div>
        <label for="country">Country:</label>
        <input type="text" id="country" v-model="user.country" />
      </div>
      <div>
        <label for="city">City:</label>
        <input type="text" id="city" v-model="user.city" />
      </div>
      <button type="submit">Register</button>
    </form>
    <div v-if="registrationMessage">
      <h2>{{ registrationMessage }}</h2>
    </div>
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

// State for user registration
const user = ref({
  name: '',
  email: '',
  country: '',
  city: ''
})

const registrationMessage = ref('')

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
    const response = await $fetch('https://mohiththarun.azurewebsites.net/api/quotes')
    quote.value = response.quote
  } catch (error) {
    console.error('Error fetching quote:', error)
  }
}

// Function to register a new user
const registerUser = async () => {
  try {
    const response = await $fetch('https://mohiththarun.azurewebsites.net/api/user', {
      method: 'POST',
      body: JSON.stringify(user.value),
      headers: {
        'Content-Type': 'application/json'
      }
    })
    registrationMessage.value = 'User registered successfully!'
  } catch (error) {
    console.error('Error registering user:', error)
    registrationMessage.value = 'Failed to register user.'
  }
}

// Fetch a quote when the component is mounted
onMounted(() => {
  fetchQuote()
})
</script>
