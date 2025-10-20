<script setup>
import { ref } from 'vue'
import ExerciseItem from './ExerciseItem.vue'

const emit = defineEmits(['remove'])
const model = defineModel('workout')

const editWorkout = ref(true)

const toggleWorkoutEdit = () => {
  if (editWorkout.value) {
    if (!model.value.name.trim()) {
      alert('Workout name is required.')
      return
    }
  }
  editWorkout.value = !editWorkout.value
}

const removeWorkout = () => emit('remove', model.value.id)

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
        <input
          v-model="workout.name"
          placeholder="Workout name..."
          type="text"
          @keyup.enter="toggleWorkoutEdit"
          required
        />
      </template>
      <template v-else>
        <h2>{{ workout.name || 'Unnamed Workout' }}</h2>
      </template>

      <div class="button-group">
        <button @click="toggleWorkoutEdit">
          {{ editWorkout ? 'Done' : 'Edit Workout' }}
        </button>
        <button v-if="editWorkout" @click="addExercise" class="ex-btn">Add Exercise</button>
        <button v-if="editWorkout" @click="removeWorkout" class="delete-btn">Delete Workout</button>
      </div>
    </div>

    <ExerciseItem
      v-model:exercises="workout.exercises"
      :editable="editWorkout"
      @remove="removeExercise"
      @save="toggleWorkoutEdit"
    />
  </div>
</template>

<style scoped>
.workout-container {
  display: flex;
  flex-direction: column;
  margin: 2rem auto;
  gap: 1rem;
  border: 1px solid hsl(0, 0%, 71%);
  padding: 1rem;
  box-shadow: 0 2px 6px hsla(0, 0%, 0%, 0.05);
  max-width: 1200px;
  background-color: hsl(0, 0%, 98%);
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.workout-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 1rem;
}

.workout-header > h2 {
  font-size: 2rem;
}

.button-group {
  display: flex;
  gap: 0.5rem;
}

button {
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  transition: 0.2s;
  background: hsl(180, 59%, 67%);
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

button:hover {
  background: hsl(180, 59%, 36%);
  box-shadow: none;
}

input {
  padding: 6px;
  font-size: 1rem;
}

.ex-btn {
  background: hsl(133, 50%, 67%);
}

.ex-btn:hover {
  background: hsl(133, 50%, 37%);
}

.delete-btn {
  background: hsl(6, 78%, 67%);
}

.delete-btn:hover {
  background: hsl(6, 78%, 37%);
}
</style>
