<script setup>
import { ref } from 'vue'
import ExerciseItem from './ExerciseItem.vue'

const model = defineModel('workout')

const addExercise = () => {
  model.value.exercises.push({ id: crypto.randomUUID(), name: '', sets: 0, reps: 0 })
}

const editWorkout = ref(true)
const toggleWorkoutEdit = () => {
  editWorkout.value = !editWorkout.value
}
</script>

<template>
  <div v-if="editWorkout">
    <input v-model="workout.name" placeholder="Name..." type="text" />
    <button @click="toggleWorkoutEdit">Done</button>
  </div>
  <div v-else>
    <p>{{ workout.name }}</p>
    <button @click="toggleWorkoutEdit">Edit</button>
  </div>

  <!-- <input v-model="workout.name" placeholder="Name..." type="text" /> -->
  <ExerciseItem
    v-for="(exercise, index) in workout.exercises"
    :key="exercise.id"
    v-model:exercises="workout.exercises[index]"
  />
  <button @click="addExercise">Add Exercise</button>
</template>

<style scoped></style>
