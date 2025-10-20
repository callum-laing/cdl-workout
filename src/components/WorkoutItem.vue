<script setup>
import { ref } from 'vue'
import ExerciseItem from './ExerciseItem.vue'

const emit = defineEmits(['remove'])
const model = defineModel('workout')

const editWorkout = ref(true)
const toggleWorkoutEdit = () => {
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
        <input v-model="workout.name" placeholder="Workout name..." type="text" />
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
    />
  </div>
</template>

<style scoped>
.workout-container {
  display: flex;
  flex-direction: column;
  margin: 2rem auto;
  gap: 1rem;
  border: 1px solid rgb(180, 180, 180);
  padding: 1rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  max-width: 1200px;
  background-color: rgb(250, 250, 250);
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
  background: rgba(79, 209, 209);
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

button:hover {
  background: rgba(59, 189, 189);
  box-shadow: none;
}

input {
  padding: 6px;
  font-size: 1rem;
}

.ex-btn {
  background: rgb(91, 201, 115);
}

.ex-btn:hover {
  background: rgb(71, 181, 95);
}

.delete-btn {
  background: #e74c3c;
}

.delete-btn:hover {
  background: #c0392b;
}
</style>
