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
  padding-bottom: 10px;
}

h1 {
  text-align: center;
  font-size: 3rem;
  letter-spacing: 2px;
  font-family: 'Oswald', sans-serif;
  font-weight: 400;
  font-style: normal;
}

@media (max-width: 722px) {
}
</style>
