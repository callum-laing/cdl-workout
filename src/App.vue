<script setup></script>

<template>
  <header>
    <h1>CDL Workout</h1>
  </header>
  <RouterView />
</template>

<style scoped>
.wrapper {
  font-size: 1.2em;
}

header {
  background-color: #606c38;
  padding-bottom: 10px;
  border-bottom: 2px solid #dda15e;
  color: white;
}

h1 {
  text-align: center;
  font-size: 3rem;
  letter-spacing: 2px;
  font-family: 'Oregano', cursive;
  font-weight: 400;
  font-style: normal;
}

@media (max-width: 722px) {
}
</style>
