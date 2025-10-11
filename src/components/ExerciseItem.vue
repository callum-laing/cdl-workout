<script setup>
const model = defineModel('exercises')
</script>

<template>
  <div>{{ exercises }}</div>
  <input v-model="exercises.name" placeholder="Exercise name.." type="text" />
  <input v-model="exercises.sets" placeholder="Sets" type="number" />
  <input v-model="exercises.reps" placeholder="Reps" type="number" />

  <div class="container">
    <table>
      <tr>
        Exercise Name:
        <td>{{ exercises.name }}</td>
      </tr>
      <tr>
        Sets:
        <td>{{ exercises.sets }}</td>
      </tr>

      <tr>
        Reps:
        <td>{{ exercises.reps }}</td>
      </tr>
    </table>
  </div>
</template>

<style scoped></style>
