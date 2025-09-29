<script setup>
import { ref } from 'vue'

defineProps({ days: Array })
const emit = defineEmits(['remove-exercise'])

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
            <th v-if="editingDay === dayIndex">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(ex, exIndex) in day.exercises" :key="exIndex">
            <td>{{ ex.name }}</td>
            <td>{{ ex.sets }}</td>
            <td>{{ ex.reps[0] }}</td>
            <td v-if="editingDay === dayIndex">
              <button @click="emit('remove-exercise', dayIndex, exIndex)">❌</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.summary {
  margin-top: 2rem;
}

.dayHeader {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 0.5rem;
  margin-bottom: 3rem;
}

th {
  font-weight: bold;
}

th,
td {
  border: 1px solid #888;
  padding: 5px 10px;
  text-align: left;
}

.editBtn {
  padding: 3px 6px;
  border: 1px solid black;
  box-shadow: 1px 1px 2px black;
  background: transparent;
  color: black;
}

.editBtn:hover {
  cursor: pointer;
  background: #eee;
}

button {
  padding: 2px 5px;
}
button:hover {
  cursor: pointer;
}
</style>
