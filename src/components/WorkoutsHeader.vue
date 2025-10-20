<template>
  <div class="header">
    <button class="dayBtn" @click="$emit('add-day')">Add Day</button>
  </div>
</template>

<script setup></script>

<style scoped>
.header {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-top: 2rem;
}

.dayBtn {
  padding: 7px;
  font-weight: bold;
  background-color: #ffff;
  border: 1px solid rgb(160, 160, 160);
  color: black;
}

.dayBtn:hover {
  box-shadow: none;
  cursor: pointer;
}
</style>
