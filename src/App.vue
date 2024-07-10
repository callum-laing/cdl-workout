<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <h1>Gym Tracker</h1>
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/nutrition">Nutrition</RouterLink>
      <RouterLink to="/workouts">Workouts</RouterLink>
    </nav>
  </header>
  <RouterView />
</template>

<style scoped>
.wrapper {
  font-size: 1.2em;
}

nav {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.2rem;
}

h1 {
  text-align: center;
  font-size: 5rem;
  padding-top: 25px;
}
</style>
