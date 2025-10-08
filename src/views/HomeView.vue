<script setup>
import { ref, onMounted, watch } from 'vue'
import WorkoutsHeader from '@/components/WorkoutsHeader.vue'
import DayManager from '@/components/DayManager.vue'
import DaySummary from '@/components/DaySummary.vue'

const days = ref([])

// --- LocalStorage Sync ---
const saveDaysToLocalStorage = () => {
  localStorage.setItem('days', JSON.stringify(days.value))
}
const loadDaysFromLocalStorage = () => {
  const savedDays = localStorage.getItem('days')
  if (savedDays) days.value = JSON.parse(savedDays)
}
onMounted(loadDaysFromLocalStorage)
watch(days, saveDaysToLocalStorage, { deep: true })

// --- Methods ---
const addDay = () => {
  days.value.push({
    name: '',
    nameSet: false,
    exercise: { name: '', sets: null, reps: [null] },
    exercises: [],
    showExerciseInputs: false
  })
}

const capitalizeFirstLetter = (string) => string.charAt(0).toUpperCase() + string.slice(1)

const saveDayName = (index) => {
  if (days.value[index].name.trim()) {
    days.value[index].name = capitalizeFirstLetter(days.value[index].name.trim()) + ' Day'
    days.value[index].nameSet = true
  }
}

const toggleExerciseInputs = (index) => {
  days.value[index].showExerciseInputs = !days.value[index].showExerciseInputs
}

const addRep = (dayIndex) => {
  days.value[dayIndex].exercise.reps.push(null)
}

const removeRep = (dayIndex, repIndex) => {
  days.value[dayIndex].exercise.reps.splice(repIndex, 1)
}

const removeDay = (index) => {
  days.value.splice(index, 1)
}

const saveExercise = (index) => {
  const exercise = days.value[index].exercise
  if (exercise.name.trim() && exercise.sets > 0 && exercise.reps.every((rep) => rep > 0)) {
    exercise.name = capitalizeFirstLetter(exercise.name.trim())
    days.value[index].exercises.push({
      name: exercise.name,
      sets: exercise.sets,
      reps: exercise.reps.slice()
    })
    days.value[index].exercise = { name: '', sets: null, reps: [null] }
    days.value[index].showExerciseInputs = false
  }
}

const removeExercise = (dayIndex, exerciseIndex) => {
  days.value[dayIndex].exercises.splice(exerciseIndex, 1)
}
</script>

<template>
  <div class="container">
    <WorkoutsHeader @add-day="addDay" />

    <div class="content">
      <DayManager
        :days="days"
        @save-day-name="saveDayName"
        @remove-day="removeDay"
        @toggle-exercise-inputs="toggleExerciseInputs"
        @remove-rep="removeRep"
        @add-rep="addRep"
        @save-exercise="saveExercise"
      />

      <DaySummary :days="days" @remove-exercise="removeExercise" />
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 3rem auto;
  max-width: 700px;
  width: 100%;
  padding: 0 1rem;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin-top: 2rem;
  gap: 2rem;
}

li {
  list-style-type: none;
}

button {
  color: black;
}
button:hover {
  cursor: pointer;
}

/* --- Responsive Layout --- */
@media (max-width: 768px) {
  .container {
    margin-top: 2rem;
  }
  .content {
    gap: 1.5rem;
  }
}
</style>
