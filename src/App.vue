<script setup>
import { ref } from 'vue'
import WorkoutItem from './components/WorkoutItem.vue'
import { watch } from 'vue'

const workouts = ref([])

const saved = localStorage.getItem('workouts')
if (saved) {
  workouts.value = JSON.parse(saved)
}

const addWorkout = () => {
  workouts.value.push({ id: crypto.randomUUID(), name: '', exercises: [] })
}

const removeWorkout = (id) => {
  workouts.value = workouts.value.filter((w) => w.id !== id)
}

watch(
  workouts,
  (newVal) => {
    localStorage.setItem('workouts', JSON.stringify(newVal))
  },
  { deep: true }
)
</script>

<template>
  <header>
    <h1>CDL Workout</h1>
  </header>

  <!-- Main View (if no workout exists) -->
  <div v-if="workouts.length === 0" class="empty-state">
    <p>You have no workouts yet.</p>
    <p>Click <span class="workout-text">Add Workout</span> to create your first one.</p>

    <div class="add-workout-center">
      <button @click="addWorkout">Add Workout</button>
    </div>
  </div>

  <!-- Main View (if a workout exists) -->
  <template v-else>
    <div class="add-workout-container">
      <button @click="addWorkout">Add Workout</button>
    </div>

    <workout-item
      v-for="(workout, index) in workouts"
      :workout="workout"
      @update:workout="($event) => (workouts[index] = $event)"
      @remove="removeWorkout"
    />
  </template>
</template>

<style scoped>
.empty-state {
  text-align: center;
  margin-top: 3rem;
  padding: 2rem;
  border: 2px solid hsl(210 20% 80%);
  background: hsl(210 20% 97%);
  color: hsl(210 10% 30%);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  line-height: 2;
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.workout-text {
  font-weight: 900;
  font-size: 1.1rem;
}

.add-workout-center {
  display: flex;
  justify-content: center;
  margin-top: 1.5rem;
}

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

/* Tablets */
@media (max-width: 1024px) {
  .add-workout-container {
    justify-content: center;
    margin: 1rem 0;
  }

  .empty-state {
    margin: 2rem 1rem;
    padding: 1.5rem;
  }

  h1 {
    font-size: 2.5rem;
  }
}

/* Phones */
@media (max-width: 600px) {
  h1 {
    font-size: 2rem;
  }

  button {
    margin: 1rem auto;
    padding: 8px 16px;
    font-size: 0.9rem;
  }

  .add-workout-container {
    justify-content: center;
    margin: 1rem 0;
  }

  .empty-state {
    margin: 2rem 1rem;
    padding: 1.25rem;
    font-size: 0.95rem;
  }

  .workout-text {
    font-size: 1rem;
  }
}
</style>
