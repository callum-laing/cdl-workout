<script setup>
const model = defineModel('exercises')

const props = defineProps({
  editable: Boolean
})

const emit = defineEmits(['remove'])
</script>

<template>
  <div class="exercises-container">
    <table>
      <thead>
        <tr>
          <th>Exercise</th>
          <th>Sets</th>
          <th>Reps</th>
          <th v-if="editable">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(exercise, index) in exercises" :key="exercise.id">
          <td>
            <template v-if="editable">
              <input v-model="exercise.name" placeholder="Exercise name..." />
            </template>
            <template v-else>
              {{ exercise.name }}
            </template>
          </td>
          <td>
            <template v-if="editable">
              <input v-model.number="exercise.sets" type="number" placeholder="Sets" />
            </template>
            <template v-else>
              {{ exercise.sets }}
            </template>
          </td>
          <td>
            <template v-if="editable">
              <input v-model.number="exercise.reps" type="number" placeholder="Reps" />
            </template>
            <template v-else>
              {{ exercise.reps }}
            </template>
          </td>
          <td v-if="editable" class="actions">
            <button class="remove-btn" @click="emit('remove', index)">✕</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.exercises-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
}

th,
td {
  border-bottom: 1px solid #ccc;
  padding: 8px;
}

input {
  width: 100%;
  padding: 4px;
}

.remove-btn {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
}

.remove-btn:hover {
  background: #c0392b;
}
</style>
