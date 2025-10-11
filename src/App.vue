<script setup>
import { ref } from 'vue'
import WorkoutItem from './components/WorkoutItem.vue'

const workouts = ref([])

const addWorkout = () => {
  workouts.value.push({ id: crypto.randomUUID(), name: '', exercises: [] })
}
</script>

<template>
  <header>
    <h1>CDL Workout</h1>
  </header>
  <workout-item
    v-for="(workout, index) in workouts"
    :workout="workout"
    @update:workout="($event) => (workouts[index] = $event)"
  />
  <button @click="addWorkout">Add Workouts</button>
</template>

<style scoped>
.wrapper {
  font-size: 1.2em;
}

h1 {
  text-align: center;
  font-size: 3rem;
  letter-spacing: 2px;
  font-family: 'Oswald', sans-serif;
  font-weight: 400;
  font-style: normal;
}
</style>
