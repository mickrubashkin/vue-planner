<script setup>
defineProps(['current'])

const emit = defineEmits(['filterChange'])

function updateFilter(by) {
  emit('filterChange', by)
}
</script>

<template>
  <nav class="filter-nav">
    <button
      @click="updateFilter('all')"
      :class="{ active: current === 'all' }"
    >
      Vue All
    </button>
    <button
      @click="updateFilter('completed')"
      :class="{ active: current === 'completed' }"
    >
      Completed
    </button>
    <button
      @click="updateFilter('ongoing')"
      :class="{ active: current === 'ongoing' }"
    >
      Ongoing
    </button>
  </nav>
</template>

<style scoped>
.filter-nav button {
  background: none;
  border: none;
  color: #bbb;
  outline: none;
  font-size: 12px;
  text-transform: uppercase;
  margin-right: 10px;
  letter-spacing: 1px;
  font-weight: bold;
  cursor: pointer;
}

.filter-nav button.active {
  color: #555;
}
</style>
