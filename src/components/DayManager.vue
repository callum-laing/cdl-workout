<script setup>
defineProps({ days: Array })
</script>

<template>
  <div>
    <div v-for="(day, index) in days" :key="index" class="day">
      <!-- Day Name Input -->
      <input
        v-if="!day.nameSet"
        v-model="day.name"
        @change="$emit('save-day-name', index)"
        placeholder="Enter day name"
        class="dayInput"
      />
      <button v-if="!day.nameSet" @click="$emit('remove-day', index)" class="cancelDayBtn">
        ❌
      </button>

      <!-- Day Section -->
      <div class="dayWrapper" v-if="day.nameSet">
        <div class="dayHeader">
          <span class="dayTitle">{{ day.name }}</span>
          <div class="actions">
            <button class="exRemoveBtn" @click="$emit('remove-day', index)">Delete Day</button>
            <button class="addExBtn" @click="$emit('toggle-exercise-inputs', index)">
              {{ day.showExerciseInputs ? 'Cancel' : 'Add Exercise' }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dayWrapper {
  background: #ffff;
  padding: 10px;
  border: 1px solid rgb(180, 180, 180);
  border-radius: 5px;
  margin-bottom: 50px;
}

.dayHeader {
  text-align: center;
}

.dayTitle {
  font-weight: bold;
  font-size: 1.2rem;
}

.actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 5px;
}

button {
  padding: 5px 10px;
  background: #fff;
  border: 1px solid #333;
  font-weight: bold;
  color: black;
}

button:hover {
  cursor: pointer;
  background: #eee;
}

.cancelDayBtn,
.exRemoveBtn {
  background: transparent;
  border: 1px solid black;
  font-size: 1rem;
}
.cancelDayBtn:hover,
.exRemoveBtn:hover {
  color: red;
}

.addExBtn {
  border: 1px solid #333;
}
</style>
