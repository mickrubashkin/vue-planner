<script setup>
import { ref } from 'vue'

const props = defineProps({
  project: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['delete', 'complete'])

const showDetails = ref(false)
const uri = 'http://localhost:3000/projects/' + props.project.id

async function deleteProject() {
  try {
    await fetch(uri, { method: 'DELETE' })
    emit('delete', props.project.id)
  } catch (e) {
    console.log(e)
  }
}

async function toggleComplete() {
  try {
    await fetch(uri, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ complete: !props.project.complete }),
    })
    emit('complete', props.project.id)
  } catch (e) {
    console.log(e)
  }
}
</script>

<template>
  <div
    :class="{ complete: project.complete }"
    class="project"
  >
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <RouterLink :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons"> edit </span>
        </RouterLink>
        <span
          @click="deleteProject"
          class="material-icons"
        >
          delete
        </span>
        <span
          @click="toggleComplete"
          class="material-icons tick"
        >
          done
        </span>
      </div>
    </div>
    <div
      v-show="showDetails"
      class="details"
    >
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<style scoped>
.project {
  margin: 20px auto;
  background-color: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}

.project.complete {
  border-left: 4px solid #00ce89;
}

.project.complete .tick {
  color: #00ce89;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.material-icons:hover {
  color: #777;
}
</style>
