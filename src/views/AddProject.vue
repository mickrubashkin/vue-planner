<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const title = ref('')
const details = ref('')

async function handleSubmit() {
  const newProject = {
    title: title.value,
    details: details.value,
    complete: false,
  }

  try {
    await fetch('http://localhost:3000/projects', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(newProject),
    })

    router.push('/')
  } catch (e) {
    console.log(e)
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input
      v-model="title"
      type="text"
      required
    />
    <label>Details:</label>
    <textarea
      v-model="details"
      required
    />
    <button>Add Project</button>
  </form>
</template>

<style scoped>
form {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
}

label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}

input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
}

textarea {
  padding: 10px;
  border: 1px solid #ddd;
  width: 100%;
  height: 100px;
}

form button {
  display: block;
  margin: 20px auto 0;
  background-color: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
}
</style>
