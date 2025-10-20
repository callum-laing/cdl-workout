<script setup>
import { ref } from 'vue'
import WorkoutItem from './components/WorkoutItem.vue'

const workouts = ref([])

const addWorkout = () => {
  workouts.value.push({ id: crypto.randomUUID(), name: '', exercises: [] })
}

const removeWorkout = (id) => {
  workouts.value = workouts.value.filter((w) => w.id !== id)
}
</script>

<template>
  <header>
    <h1>CDL Workout</h1>
  </header>
  <div class="add-workout-container">
    <button @click="addWorkout">Add Workouts</button>
  </div>

  <workout-item
    v-for="(workout, index) in workouts"
    :workout="workout"
    @update:workout="($event) => (workouts[index] = $event)"
    @remove="removeWorkout"
  />
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

button {
  padding: 5px;
  margin: 2rem;
  background: hsl(203, 93%, 57%);
  border: 1px solid hsl(203, 93%, 57%);
  transition: 0.1s ease-in;
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

button:hover {
  cursor: pointer;
  background: hsl(203, 93%, 17%);
  border: 1px solid hsl(203, 93%, 17%);
  box-shadow: none;
  color: white;
}

.add-workout-container {
  display: flex;
  justify-content: flex-start;
  margin: 1rem 20.5rem;
}
</style>
