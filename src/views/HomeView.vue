<script setup></script>

<template>
  <main>
    <p>
      <span>PLEASE READ:</span> Once the Nutrition and Workout pages are built, the home page will
      showcase a preview of the 2 pages together for users to get an insight into the app before
      they build it themselves. For now, enjoy a photo of my dog, Fleur!
    </p>
    <img src="../assets/doggy.jpg" />
  </main>
</template>

<style scoped>
main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 20px;
  padding-top: 20px;
}

img {
  border-radius: 5px;
}

p {
  font-size: 2rem;
}

span {
  color: yellow;
  font-weight: bold;
}
</style>
