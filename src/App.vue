<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <h1>CDL Workout</h1>
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/workouts">Workouts</RouterLink>
      <RouterLink to="/nutrition">Nutrition</RouterLink>
    </nav>
  </header>
  <RouterView />
</template>

<style scoped>
.wrapper {
  font-size: 1.2em;
}

header {
  background-color: rgba(10, 10, 10, 0.2);
  padding-bottom: 10px;
}

nav {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.2rem;
  font-family: 'Caveat', cursive;
}

nav > a {
  text-decoration: none;
  color: white;
  font-size: 1.4rem;
}

nav > a:hover {
  color: rgba(200, 100, 100);
}
h1 {
  text-align: center;
  font-size: 5rem;
  padding-top: 25px;
  font-family: 'Indie Flower', cursive;
}
</style>
