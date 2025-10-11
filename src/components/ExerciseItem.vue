<script setup>
import { ref } from 'vue'
const model = defineModel('exercises')

const editExercises = ref(true)

const toggleEdit = () => {
  editExercises.value = !editExercises.value
}
</script>

<template>
  <div v-if="editExercises">
    <input v-model="exercises.name" placeholder="Exercise name.." type="text" />
    <input v-model="exercises.sets" placeholder="Sets" type="number" />
    <input v-model="exercises.reps" placeholder="Reps" type="number" />
    <button @click="toggleEdit">Done</button>
  </div>

  <div v-else>
    <p>{{ exercises.name }}</p>
    <p>{{ exercises.sets }}</p>
    <p>{{ exercises.reps }}</p>
    <button @click="toggleEdit">Edit</button>
  </div>
</template>

<style scoped></style>
