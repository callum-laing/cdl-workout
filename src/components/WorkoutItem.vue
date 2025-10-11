<script setup>
import ExerciseItem from './ExerciseItem.vue'

const model = defineModel('workout')

const addExercise = () => {
  model.value.exercises.push({ id: crypto.randomUUID(), name: '', sets: 0, reps: 0 })
}
</script>

<template>
  <div>{{ workout }}</div>
  <input v-model="workout.name" placeholder="Name..." type="text" />
  <ExerciseItem
    v-for="(exercise, index) in workout.exercises"
    :key="exercise.id"
    v-model:exercises="workout.exercises[index]"
  />
  <button @click="addExercise">Add Exercise</button>
</template>

<style scoped></style>
