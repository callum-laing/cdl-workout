<script>
import { ref, onMounted, watch } from 'vue'

export default {
  setup() {
    const days = ref([])

    const saveDaysToLocalStorage = () => {
      localStorage.setItem('days', JSON.stringify(days.value))
    }

    const loadDaysFromLocalStorage = () => {
      const savedDays = localStorage.getItem('days')
      if (savedDays) {
        days.value = JSON.parse(savedDays)
      }
    }

    onMounted(() => {
      loadDaysFromLocalStorage()
    })

    watch(days, saveDaysToLocalStorage, { deep: true })

    const addDay = () => {
      days.value.push({
        name: '',
        nameSet: false,
        exercise: { name: '', sets: null, reps: [null] },
        exercises: [],
        showExerciseInputs: false
      })
    }

    const capitalizeFirstLetter = (string) => {
      return string.charAt(0).toUpperCase() + string.slice(1)
    }

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
          reps: exercise.reps.slice() // Copy array to avoid reference issues
        })
        days.value[index].exercise = { name: '', sets: null, reps: [null] }
        days.value[index].showExerciseInputs = false
      }
    }

    const removeExercise = (dayIndex, exerciseIndex) => {
      days.value[dayIndex].exercises.splice(exerciseIndex, 1)
    }

    return {
      days,
      addDay,
      capitalizeFirstLetter,
      saveDayName,
      toggleExerciseInputs,
      addRep,
      removeRep,
      removeDay,
      saveExercise,
      removeExercise
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="header">
      <p class="workoutTitle">Workouts</p>
      <button class="dayBtn" @click="addDay">Add Day</button>
    </div>
    <div class="wrapper">
      <div class="leftColumn">
        <div v-for="(day, index) in days" :key="index" class="day">
          <input
            v-model="day.name"
            @change="saveDayName(index)"
            placeholder="Enter day name"
            v-if="!day.nameSet"
          />
          <button v-if="!day.nameSet" @click="removeDay(index)" class="cancelDayBtn">❌</button>
          <div class="dayWrapper">
            <span v-if="day.nameSet"
              >{{ day.name }}

              <button class="exRemoveBtn" v-if="day.nameSet" @click="removeDay(index)">❌</button>
            </span>
            <button v-if="day.nameSet" class="addExBtn" @click="toggleExerciseInputs(index)">
              {{ day.showExerciseInputs ? 'Cancel' : 'Add Exercise' }}
            </button>
            <div v-if="day.showExerciseInputs">
              <div>
                <label for="nameBox">Exercise Name: </label>
                <input class="nameBox inputBox" v-model="day.exercise.name" />
                <br />
                <label for="setBox">Sets: </label>
                <input class="setBox inputBox" type="number" v-model.number="day.exercise.sets" />
                <br />
                <label for="repBox">Reps: </label>
                <div v-for="(rep, repIndex) in day.exercise.reps" :key="repIndex">
                  <input
                    class="repBox inputBox"
                    type="number"
                    v-model.number="day.exercise.reps[repIndex]"
                  />
                  <button class="repRemoveBtn" @click="removeRep(index, repIndex)">Remove</button>
                </div>
                <button class="addExBtn" @click="addRep(index)">Add More Reps</button>
                <br />
                <button class="exBtn" @click="saveExercise(index)">Complete Setup ✓</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="rightColumn">
        <div v-for="(day, dayIndex) in days" :key="day.name" class="summary">
          <h2 class="exListDay">{{ day.name }}</h2>
          <ul>
            <li v-for="(exercise, exerciseIndex) in day.exercises" :key="exercise.name">
              {{ exercise.name }} {{ exercise.sets }}x{{ exercise.reps.join(', ') }}
              <button class="exRemoveBtn" @click="removeExercise(dayIndex, exerciseIndex)">
                ❌
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.workoutTitle {
  font-size: 2rem;
  margin-top: 20px;
  font-family: 'Oregano', cursive;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 2px;
}

.container {
  padding-left: 100px;
}

.day {
  margin: 20px 0;
}

.exListDay {
  font-size: 2rem;
  text-decoration: underline;
  margin-bottom: 5px;
  font-family: 'Oregano', cursive;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 2px;
}

.wrapper {
  display: flex;
  flex-direction: row;
  gap: 20px;
  margin: 20px;
}

.leftColumn,
.rightColumn {
  flex: 1;
}

.dayWrapper {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background-color: rgb(255, 194, 49, 0.2);
  border: 2px solid black;
  padding: 10px;
  width: 500px;
  border-radius: 5px;
  box-shadow: 1px 1px 1px black;
}

.summary {
  margin-bottom: 50px;
}

input {
  margin-right: 20px;
  margin-bottom: 20px;
  padding: 5px;
  border: 2px solid rgba(50, 50, 50);
  background-color: rgba(100, 100, 100, 0.4);
  box-shadow: 1px 1px 1px black;
  border-radius: 5px;
  color: black;
}

.dayBtn {
  padding: 5px;
  padding: 5px;
  font-weight: bold;
  background-color: transparent;
  border: 2px solid black;
  box-shadow: 1px 1px 1px black;
}

.dayBtn:hover {
  box-shadow: none;
}

.cancelDayBtn {
  width: 100px;
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.cancelDayBtn:hover {
  border: 2px solid red;
  cursor: pointer;
}

.addExBtn {
  padding: 5px;
  margin: 5px;
  font-weight: bold;
  background-color: transparent;
  border: 2px solid black;
  box-shadow: 1px 1px 1px black;
}

.addExBtn:hover {
  box-shadow: none;
}
.removeDayBtn {
  width: 100px;
  background-color: red;
  border: none;
  margin-top: 10px;
}

.exBtn {
  padding: 5px;
  font-weight: bold;
  background-color: transparent;
  border: 2px solid black;
  box-shadow: 1px 1px 1px black;
}

.exBtn:hover {
  color: green;
  cursor: pointer;
  box-shadow: none;
}

.exRemoveBtn {
  background-color: transparent;
  border: none;
}

.repRemoveBtn {
  padding: 5px;
  font-weight: bold;
  background-color: transparent;
  border: 2px solid black;
  box-shadow: 1px 1px 1px black;
}

.repRemoveBtn:hover {
  box-shadow: none;
  color: red;
}

.setBox,
.repBox {
  width: 100px;
}

li {
  list-style-type: none;
}

button {
  color: black;
  border-radius: 5px;
}
button:hover {
  cursor: pointer;
}

@media (max-width: 768px) {
  .wrapper {
    flex-direction: column;
    align-items: center;
  }
  .dayWrapper {
    margin: 0;
    padding: 1em;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .wrapper {
    flex-direction: column;
    align-items: center;
  }
}
</style>
