<template>
  <div class="header">
    <p class="workoutTitle">Workouts</p>
    <button class="dayBtn" @click="$emit('add-day')">Add Workout</button>
  </div>
</template>

<script setup></script>

<style scoped>
.workoutTitle {
  font-size: 2rem;
  margin-top: 20px;
  font-family: 'Oregano', cursive;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 2px;
}

.dayBtn {
  padding: 7px;
  font-weight: bold;
  background-color: transparent;
  border: 1px solid black;
  box-shadow: 1px 1px 1px black;
  color: black;
}

.dayBtn:hover {
  box-shadow: none;
  cursor: pointer;
}
</style>
