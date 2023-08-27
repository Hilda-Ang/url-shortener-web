<template>
  <div class="container">
    <h1>URL Shortener</h1>
    <form @submit.prevent="submit">
      <div class="row">
        <input v-model="longUrlInput" placeholder="Input the URL to shorten" />
      </div>
      <div class="row">
        <p>{{ SERVER_ADDRESS }}/</p>
        <input v-model="shortUrlInput" placeholder="Input short URL path (optional)" />
      </div>
      <div class="row button">
        <button type="submit">Submit</button>
      </div>
      <div class="row">
        <p v-if="shortUrl">Short URL created: <a :href="'http://' + shortUrl" target="_blank">http://{{ shortUrl }}</a>
        </p>
        <p v-else-if="error">Error creating short URL: {{ error }}</p>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'

import { SERVER_ADDRESS } from '@/constants'

const shortUrl = ref("")
const error = ref("")
const longUrlInput = ref("")
const shortUrlInput = ref("")

const submit = async () => {
  const response = await fetch(SERVER_ADDRESS + "/shorten", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      longUrl: longUrlInput.value,
      shortUrl: shortUrlInput.value
    })
  })
  const { shortUrl: shortUrlRes, error: errorRes } = await response.json()
  if (shortUrlRes) {
    shortUrl.value = shortUrlRes
  } else if (errorRes) {
    error.value = errorRes
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  background-color: #2c3e50;
}

h1 {
  color: #42b883;
  font-size: 48px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;
}

.row {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 100%;
  margin: 12px 0px;
  font-size: 18px;
  color: lightgray;
  height: 60px;
}

input {
  box-sizing: border-box;
  height: 100%;
  flex-grow: 1;
  padding: 8px;
  border: 2px solid #42b883;
  border-radius: 8px;
  font-size: 18px;
  background-color: #2c3e50;
  color: lightgray;
}

input:focus {
  outline: none;
}

p {
  margin-right: 8px;
}

.button {
  margin-top: 32px;
}

button {
  height: 100%;
  max-width: 20%;
  flex-grow: 1;
  border: 2px solid #42b883;
  border-radius: 8px;
  font-size: 20px;
  background-color: #42b883;
  color: #2c3e50;
}

a {
  color: lightgray;
}
</style>
