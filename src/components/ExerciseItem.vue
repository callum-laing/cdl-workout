<script setup>
const model = defineModel('exercises')
</script>

<template>
  <div>{{ exercises }}</div>
  <input v-model="exercises.name" placeholder="Exercise name.." type="text" />
  <input v-model="exercises.sets" placeholder="Sets" type="number" />
  <input v-model="exercises.reps" placeholder="Reps" type="number" />

  <p>{{ exercises.name }}</p>
  <p>{{ exercises.sets }}</p>
  <p>{{ exercises.reps }}</p>
</template>

<style scoped></style>
