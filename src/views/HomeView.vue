<script setup>
import { onMounted, ref } from 'vue'
import SingleProject from '@/components/SingleProject.vue'

const projects = ref([])

function handleDelete(id) {
  projects.value = projects.value.filter((project) => project.id !== id)
}

function handleComplete(id) {
  const project = projects.value.find((project) => project.id === id)
  project.complete = !project.complete
}

onMounted(async () => {
  try {
    const response = await fetch('http://localhost:3000/projects')
    const data = await response.json()
    projects.value = data
  } catch (e) {
    console.log(e)
  }
})
</script>

<template>
  <div class="home">
    <div v-if="projects.length">
      <div
        v-for="project in projects"
        :key="project.id"
      >
        <SingleProject
          @delete="handleDelete"
          @complete="handleComplete"
          :project="project"
        />
      </div>
    </div>
  </div>
</template>
