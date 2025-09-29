<script setup>
defineProps({ days: Array })
</script>

<template>
  <div>
    <div v-for="(day, index) in days" :key="index" class="day">
      <!-- Day Name Input -->
      <input
        v-if="!day.nameSet"
        v-model="day.name"
        @change="$emit('save-day-name', index)"
        placeholder="Enter day name"
        class="dayInput"
      />
      <button v-if="!day.nameSet" @click="$emit('remove-day', index)" class="cancelDayBtn">
        ❌
      </button>

      <!-- Day Section -->
      <div class="dayWrapper" v-if="day.nameSet">
        <div class="dayHeader">
          <span class="dayTitle">{{ day.name }}</span>
          <div class="actions">
            <button class="exRemoveBtn" @click="$emit('remove-day', index)">❌</button>
            <button class="addExBtn" @click="$emit('toggle-exercise-inputs', index)">
              {{ day.showExerciseInputs ? 'Cancel' : 'Add Exercise' }}
            </button>
          </div>
        </div>

        <!-- Exercise Input Form -->
        <div v-if="day.showExerciseInputs" class="exerciseForm">
          <div class="formColumn">
            <label>Exercise</label>
            <input class="nameBox" v-model="day.exercise.name" placeholder="e.g. Bench Press" />
          </div>

          <div class="formColumn">
            <label>Sets</label>
            <input
              class="setBox"
              type="number"
              v-model.number="day.exercise.sets"
              placeholder="4"
            />
          </div>

          <div class="formColumn">
            <label>Reps</label>
            <input
              class="repBox"
              type="number"
              v-model.number="day.exercise.reps[0]"
              placeholder="12"
            />
          </div>

          <div class="formColumn buttonCol">
            <label>&nbsp;</label>
            <button class="exBtn" @click="$emit('save-exercise', index)">Add Exercise ✓</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dayWrapper {
  margin-top: 1rem;
  background: rgba(50, 100, 200, 0.4);
  border: 1px solid rgba(50, 100, 200, 0.2);
  box-shadow: 2px 2px 2px rgba(50, 100, 200, 0.8);
  padding: 10px 15px;
}

.dayHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.day {
  margin-top: 10px;
}

.dayTitle {
  font-weight: bold;
  font-size: 1.2rem;
}

.actions {
  display: flex;
  gap: 0.5rem;
}

/* New layout for exercise inputs */
.exerciseForm {
  display: grid;
  grid-template-columns: 1fr 80px 80px auto;
  gap: 1rem;
  margin-top: 0.75rem;
}

.formColumn {
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.85rem;
  font-weight: bold;
  margin-bottom: 4px;
  color: black;
}

input {
  padding: 6px;
  border: 1px solid black;
  background: rgba(250, 250, 250, 0.8);
  color: black;
}

.nameBox {
  min-width: 150px;
}
.setBox,
.repBox {
  width: 80px;
}

button {
  padding: 5px 10px;
  background: #fff;
  border: 1px solid #333;
  font-weight: bold;
  color: black;
}

button:hover {
  cursor: pointer;
  background: #eee;
}

.cancelDayBtn,
.exRemoveBtn {
  background: transparent;
  border: none;
  font-size: 1rem;
}
.cancelDayBtn:hover,
.exRemoveBtn:hover {
  color: red;
}

.addExBtn {
  border: 1px solid #333;
}

.exBtn {
  border: 1px solid #333;
  color: green;
}

.buttonCol {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
</style>
