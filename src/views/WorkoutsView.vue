<template>
  <div class="wrapper">
    <h1>Workouts</h1>
    <button class="dayBtn" @click="addDay">Add Day</button>
    <div v-for="(day, index) in days" :key="index" class="day">
      <input
        v-model="day.name"
        @change="saveDayName(index)"
        placeholder="Enter day name"
        v-if="!day.nameSet"
      />
      <button v-if="!day.nameSet" @click="removeDay(index)" class="cancelDayBtn">❌</button>
      <div class="dayWrapper">
        <span v-if="day.nameSet">{{ day.name }}</span>
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
              <button @click="removeRep(index, repIndex)">Remove</button>
            </div>
            <button @click="addRep(index)">Add More Reps</button>
            <br />
            <button class="exBtn" @click="saveExercise(index)">Complete Setup ✓</button>
          </div>
        </div>
        <ul>
          <p class="exListTitle">Exercises:</p>
          <li v-for="(exercise, exIndex) in day.exercises" :key="exIndex">
            {{ exercise.name }}: {{ exercise.sets }} x {{ exercise.reps.join(', ') }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      days: []
    }
  },
  methods: {
    addDay() {
      this.days.push({
        name: '',
        nameSet: false,
        exercise: { name: '', sets: null, reps: [null] },
        exercises: [],
        showExerciseInputs: false
      })
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1)
    },
    saveDayName(index) {
      if (this.days[index].name.trim()) {
        this.days[index].name = this.capitalizeFirstLetter(this.days[index].name.trim()) + ' Day'
        this.days[index].nameSet = true
      }
    },
    toggleExerciseInputs(index) {
      this.days[index].showExerciseInputs = !this.days[index].showExerciseInputs
    },
    addRep(dayIndex) {
      this.days[dayIndex].exercise.reps.push(null)
    },
    removeRep(dayIndex, repIndex) {
      this.days[dayIndex].exercise.reps.splice(repIndex, 1)
    },
    removeDay(index) {
      this.days.splice(index, 1)
    },
    saveExercise(index) {
      const exercise = this.days[index].exercise
      if (exercise.name.trim() && exercise.sets > 0 && exercise.reps.every((rep) => rep > 0)) {
        exercise.name = this.capitalizeFirstLetter(exercise.name.trim())
        this.days[index].exercises.push({
          name: exercise.name,
          sets: exercise.sets,
          reps: exercise.reps.slice() // Copy array to avoid reference issues
        })
        this.days[index].exercise = { name: '', sets: null, reps: [null] }
        this.days[index].showExerciseInputs = false
      }
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 2rem;
  margin-top: 20px;
}

.exListTitle {
  font-family: 'Caveat', cursive;
}

.day {
  margin: 20px 0;
}

.wrapper {
  display: flex;
  flex-direction: column;
  margin-left: 20px;
  gap: 10px;
}

.dayWrapper {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background-color: rgba(0, 0, 0, 0.3);
  padding: 10px;
  border-radius: 5px;
  width: 900px;
}

input {
  margin-right: 20px;
  margin-bottom: 20px;
  background-color: rgba(50, 50, 50, 0.4);
  border-radius: 5px;
}

.dayBtn {
  width: 100px;
}

.cancelDayBtn {
  width: 100px;
  background-color: transparent;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.cancelDayBtn:hover {
  background-color: darkred;
}

.addExBtn {
  width: 150px;
}

.exBtn {
  margin-top: 15px;
  background-color: green;
  padding: 2px;
  border-radius: 5px;
  border: none;
}

.exBtn:hover {
  background-color: white;
  color: green;
  cursor: pointer;
}

button:hover {
  cursor: pointer;
  background-color: white;
  color: black;
}

.setBox,
.repBox {
  width: 100px;
}

li {
  list-style-type: none;
}
</style>
