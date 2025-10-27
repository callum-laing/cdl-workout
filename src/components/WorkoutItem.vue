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
        <button @click="toggleWorkoutEdit" class="icon-btn">
          <template v-if="editWorkout">Done</template>
          <template v-else>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              width="18"
              height="18"
              fill="currentColor"
            >
              <path
                d="M3 17.25V21h3.75L17.81 9.94l-3.75-3.75L3 
             17.25zM20.71 7.04a1.003 1.003 0 0 0 
             0-1.42l-2.34-2.34a1.003 1.003 0 0 0-1.42 
             0l-1.83 1.83 3.75 3.75 1.84-1.82z"
              />
            </svg>
          </template>
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
  display: grid;
  margin: 2rem auto;
  gap: 1rem;
  border: 1px solid hsl(0, 0%, 71%);
  padding: 1rem;
  box-shadow: 0 2px 6px hsla(0, 0%, 0%, 0.05);
  width: 100%;
  max-width: 500px;
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

.icon-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 12px;
  color: hsl(203, 93%, 37%);
  transition: color 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-btn:hover {
  color: hsl(203, 93%, 57%);
}

.icon-btn svg {
  pointer-events: none;
  width: 26px;
  height: 100%;
}

button:not(.icon-btn) {
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  transition: 0.2s;
  background: hsl(180, 59%, 67%);
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

button:not(.icon-btn):hover {
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

/* Tablets */
@media (max-width: 1024px) {
  .workout-container {
    margin: 1rem;
    padding: 0.75rem;
  }

  .workout-header {
    flex-direction: column;
    align-items: stretch;
    text-align: center;
  }

  .button-group {
    justify-content: center;
  }

  .workout-header > h2 {
    font-size: 1.5rem;
  }

  input {
    width: 100%;
  }
}

/* Phones */
@media (max-width: 600px) {
  .workout-container {
    margin: 1rem 0.5rem;
    padding: 0.75rem;
  }

  .workout-header {
    padding: 0.5rem;
  }

  .workout-header > h2 {
    font-size: 1.25rem;
  }

  .button-group {
    flex-direction: column;
    align-items: stretch;
  }

  button {
    width: 100%;
  }

  input {
    width: 100%;
    font-size: 0.9rem;
  }
}
</style>
