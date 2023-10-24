<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import SingleProject from '@/components/SingleProject.vue'
import FilterNav from '@/components/FilterNav.vue'

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

const filter = ref('all')

const filteredProjects = computed(() => {
  if (filter.value === 'completed') {
    return projects.value.filter((project) => project.complete)
  }
  if (filter.value === 'ongoing') {
    return projects.value.filter((project) => !project.complete)
  }

  return projects.value
})
</script>

<template>
  <div class="home">
    <FilterNav
      @filterChange="filter = $event"
      :current="filter"
    />
    <div v-if="projects.length">
      <div
        v-for="project in filteredProjects"
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
