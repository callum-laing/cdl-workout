<script setup>
import { ref } from 'vue'
import ExerciseItem from './ExerciseItem.vue'

const model = defineModel('workout')

const editWorkout = ref(true)
const toggleWorkoutEdit = () => {
  editWorkout.value = !editWorkout.value
}

const addExercise = () => {
  model.value.exercises.push({
    id: crypto.randomUUID(),
    name: '',
    sets: 0,
    reps: 0
  })
}

const removeExercise = (index) => {
  model.value.exercises.splice(index, 1)
}
</script>

<template>
  <div class="workout-container">
    <div class="workout-header">
      <template v-if="editWorkout">
        <input v-model="workout.name" placeholder="Workout name..." type="text" />
      </template>
      <template v-else>
        <h2>{{ workout.name || 'Unnamed Workout' }}</h2>
      </template>

      <div class="button-group">
        <button @click="toggleWorkoutEdit">
          {{ editWorkout ? 'Done' : 'Edit Workout' }}
        </button>
        <button v-if="editWorkout" @click="addExercise">Add Exercise</button>
      </div>
    </div>

    <ExerciseItem
      v-model:exercises="workout.exercises"
      :editable="editWorkout"
      @remove="removeExercise"
    />
  </div>
</template>

<style scoped>
.workout-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  border: 1px solid #ddd;
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.workout-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.button-group {
  display: flex;
  gap: 0.5rem;
}

button {
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.2s;
}

input {
  padding: 6px;
  font-size: 1rem;
}
</style>
