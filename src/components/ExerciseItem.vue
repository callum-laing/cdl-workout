<script setup>
const model = defineModel('exercises')

const props = defineProps({
  editable: Boolean
})

const emit = defineEmits(['remove', 'save'])
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
              <input
                v-model="exercise.name"
                placeholder="Exercise name..."
                @keyup.enter="emit('save')"
              />
            </template>
            <template v-else>
              {{ exercise.name }}
            </template>
          </td>
          <td>
            <template v-if="editable">
              <input
                v-model.number="exercise.sets"
                type="number"
                placeholder="Sets"
                @keyup.enter="emit('save')"
              />
            </template>
            <template v-else>
              {{ exercise.sets }}
            </template>
          </td>
          <td>
            <template v-if="editable">
              <input
                v-model.number="exercise.reps"
                type="number"
                placeholder="Reps"
                @keyup.enter="emit('save')"
              />
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
  padding: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  border: 1px solid hsl(203, 93%, 17%);
  background-color: hsl(202, 93%, 77%);
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

th,
td {
  padding: 8px;
  text-align: center;
}

th {
  font-weight: 600;
}

td {
  border-top: 1px solid hsl(203, 93%, 17%);
  border-bottom: 1px solid hsl(203, 93%, 17%);
}

input {
  width: 100%;
  padding: 4px;
}

.remove-btn {
  background: hsl(6, 78%, 57%);
  border: none;
  font-weight: bold;
  padding: 4px 8px;
  cursor: pointer;
  box-shadow:
    rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
    rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.remove-btn:hover {
  background: hsl(6, 63%, 46%);
  box-shadow: none;
}
</style>
