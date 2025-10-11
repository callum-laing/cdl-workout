<script setup>
import { ref } from 'vue'

defineProps({ days: Array })
const emit = defineEmits(['remove-exercise', 'save-exercise', 'toggle-exercise-inputs'])

// Track which day is currently in "edit" mode
const editingDay = ref(null)

// Toggle edit mode for a day
const toggleEdit = (dayIndex) => {
  editingDay.value = editingDay.value === dayIndex ? null : dayIndex
}
</script>

<template>
  <div class="summary">
    <div v-for="(day, dayIndex) in days" :key="dayIndex" class="daySummary">
      <div class="dayHeader">
        <h3>{{ day.name }}</h3>
        <button class="editBtn" @click="toggleEdit(dayIndex)">
          {{ editingDay === dayIndex ? 'Done' : 'Edit' }}
        </button>
      </div>

      <table>
        <thead>
          <tr>
            <th>Exercise</th>
            <th>Sets</th>
            <th>Reps</th>
            <th v-if="editingDay === dayIndex || day.showExerciseInputs">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(ex, exIndex) in day.exercises" :key="exIndex">
            <td>
              <template v-if="editingDay === dayIndex">
                <input
                  v-model="day.exercises[exIndex].name"
                  class="tableInput"
                  placeholder="Exercise name"
                />
              </template>
              <template v-else>
                {{ ex.name }}
              </template>
            </td>
            <td>
              <template v-if="editingDay === dayIndex">
                <input
                  v-model.number="day.exercises[exIndex].sets"
                  type="number"
                  min="0"
                  class="tableInput numberInput"
                />
              </template>
              <template v-else>
                {{ ex.sets }}
              </template>
            </td>
            <td>
              <template v-if="editingDay === dayIndex">
                <input
                  v-model.number="day.exercises[exIndex].reps[0]"
                  type="number"
                  min="0"
                  class="tableInput numberInput"
                />
              </template>
              <template v-else>
                {{ ex.reps[0] }}
              </template>
            </td>
            <td v-if="editingDay === dayIndex" class="actionCell">
              <button @click="emit('remove-exercise', dayIndex, exIndex)">❌</button>
            </td>
            <td v-else-if="day.showExerciseInputs"></td>
          </tr>
          <tr v-if="day.showExerciseInputs" class="newExerciseRow">
            <td>
              <input v-model="day.exercise.name" class="tableInput" placeholder="Exercise name" />
            </td>
            <td>
              <input
                v-model.number="day.exercise.sets"
                type="number"
                min="0"
                class="tableInput numberInput"
                placeholder="Sets"
              />
            </td>
            <td>
              <input
                v-model.number="day.exercise.reps[0]"
                type="number"
                min="0"
                class="tableInput numberInput"
                placeholder="Reps"
              />
            </td>
            <td class="actionCell">
              <button class="primary" @click="emit('save-exercise', dayIndex)">Add</button>
              <button @click="emit('toggle-exercise-inputs', dayIndex)">Cancel</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.summary {
  border: 1px solid rgb(180, 180, 180);
  background: #fff;
  border-radius: 5px;
  padding: 2rem;
}

.dayHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: bold;
}

table {
  width: 100%;
  margin-top: 5px;
  border: 1px solid rgb(180, 180, 180);
  margin-bottom: 50px;
  background: rgba(200, 230, 250);
}

th {
  font-weight: bold;
  text-decoration: underline;
}

th,
td {
  padding: 10px 20px;
  text-align: center;
  border-bottom: 1px solid rgb(180, 180, 180);
  letter-spacing: 0.8px;
}

.tableInput {
  width: 100%;
  padding: 4px 6px;
  border: 1px solid #888;
  box-sizing: border-box;
  background: #fff;
  color: black;
}

.numberInput {
  max-width: 80px;
}

.newExerciseRow {
  background-color: #f7f7f7;
  color: black;
}

.actionCell {
  display: flex;
  gap: 0.25rem;
}

tbody tr:last-of-type td {
  border-bottom: 0;
}

.editBtn {
  padding: 3px 6px;
  border: 1px solid rgba(100, 100, 100);
  background: #ffff;
  color: black;
  background: rgba(200, 230, 250);
}

.editBtn:hover {
  cursor: pointer;
  background: #eee;
}

button {
  padding: 2px 5px;
  background: transparent;
}

button:hover {
  cursor: pointer;
}
</style>
