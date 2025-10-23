<script setup>
import { ref, watch } from 'vue'
import WorkoutItem from './components/WorkoutItem.vue'

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

watch(workouts, (newVal) => localStorage.setItem('workouts', JSON.stringify(newVal)), {
  deep: true
})
</script>

<template>
  <div class="layout">
    <header>
      <h1>CDL WORKOUT</h1>
    </header>

    <main>
      <div v-if="workouts.length === 0" class="empty-state">
        <p>You have no workouts yet.</p>
        <p>Click <span class="workout-text">Add Workout</span> to create your first one.</p>

        <div class="add-workout-center">
          <button @click="addWorkout">Add Workout</button>
        </div>
      </div>

      <template v-else>
        <div class="add-workout-container">
          <button @click="addWorkout">Add Workout</button>
        </div>

        <workout-item
          v-for="(workout, index) in workouts"
          :key="workout.id"
          :workout="workout"
          @update:workout="($event) => (workouts[index] = $event)"
          @remove="removeWorkout"
        />
      </template>
    </main>

    <footer>
      <p>© {{ new Date().getFullYear() }} Callum Laing</p>
    </footer>
  </div>
</template>

<style scoped>
.layout {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

header {
  text-align: center;
  padding: 1rem;
  color: hsl(210, 7%, 94%);
  background: hsl(210 20% 46%);
  border-bottom: 2px solid hsl(210 20% 35%);
  box-shadow:
    0 4px 8px hsla(210, 30%, 10%, 0.4),
    0 2px 4px hsla(210, 20%, 20%, 0.3);
}

h1 {
  font-size: 3rem;
  letter-spacing: 2px;
  font-family: 'Roboto', sans-serif;
  font-weight: 600;
  margin: 0;
}

main {
  padding: 2rem 0;
  overflow: visible;
}

.add-workout-container {
  display: flex;
  justify-content: center;
  margin: 1rem 20.5rem;
}

button {
  padding: 6px 12px;
  background: hsl(203, 93%, 57%);
  border: 1px solid hsl(203, 93%, 57%);
  color: white;
  font-weight: 500;
  transition: 0.2s ease-in;
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

button:hover {
  background: hsl(203, 93%, 37%);
  border-color: hsl(203, 93%, 37%);
  box-shadow: none;
  cursor: pointer;
}

.empty-state {
  text-align: center;
  margin: 3rem auto 0;
  padding: 2rem;
  border: 2px solid hsl(210 20% 80%);
  background: hsl(210 20% 97%);
  color: hsl(210 10% 30%);
  max-width: 600px;
  line-height: 1.8;
  border-radius: 10px;
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.workout-text {
  font-weight: 700;
  font-size: 1.1rem;
}

.add-workout-center {
  display: flex;
  justify-content: center;
  margin-top: 1.5rem;
}

footer {
  text-align: center;
  padding: 1rem;
  font-size: 0.9rem;
  color: hsl(210, 7%, 94%);
  background: hsl(210 20% 46%);
  border-top: 2px solid hsl(210 20% 35%);
  box-shadow:
    0 -4px 8px hsla(210, 30%, 10%, 0.4),
    0 -2px 4px hsla(210, 20%, 20%, 0.3);
}

/* === RESPONSIVE === */
@media (max-width: 1024px) {
  h1 {
    font-size: 2.5rem;
  }

  .add-workout-container {
    justify-content: center;
    margin: 1rem 0;
  }

  .empty-state {
    margin: 2rem 1rem;
    padding: 1.5rem;
  }
}

@media (max-width: 600px) {
  h1 {
    font-size: 2rem;
  }

  button {
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
