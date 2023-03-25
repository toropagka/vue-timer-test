<template>
  <div class="timer-container">
    <div class="timer-container_text" :class="{ active: isRunning }">
      {{ formatTime }}
    </div>
    <div class="timer-container_buttons">
      <button @click="toggleTimer">
        <svg
          v-if="!isRunning"
          width="17"
          height="20"
          viewBox="0 0 17 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E" />
        </svg>
        <svg
          v-else
          width="17"
          height="20"
          viewBox="2 0 10 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <rect x="7" width="3" height="20" fill="white" />
          <rect width="3" height="20" fill="white" />
        </svg>
      </button>
      <button @click="resetTimer">
        <svg
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <rect
            :class="{ active: isRunning }"
            width="20"
            height="20"
            fill="#9E9E9E"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const startTime = ref(null);
const pauseTime = ref(null);
const currentTime = ref(null);
const elapsedTime = ref(0);
const timer = ref(null);
const isRunning = ref(false);

const formatTime = computed(() => {
  const totalSeconds = Math.floor(elapsedTime.value / 1000);
  if (totalSeconds < 60) {
    // показываем только секунды, если меньше 60 секунд
    return `${totalSeconds}`;
  } else if (totalSeconds < 3600) {
    // показываем минуты и секунды, если меньше 60 минут
    const minutes = Math.floor(totalSeconds / 60);
    const seconds = totalSeconds % 60;
    return `${minutes}:${pad(seconds)}`;
  } else {
    // показываем часы, минуты и секунды, если больше или равно 60 минут
    const hours = Math.floor(totalSeconds / 3600);
    const totalMinutes = Math.floor((totalSeconds - hours * 3600) / 60);
    const seconds = totalSeconds % 60;
    return `${hours}:${pad(totalMinutes)}:${pad(seconds)}`;
  }
});

function pad(value) {
  return value.toString().padStart(2, '0');
}

const toggleTimer = () => {
  if (!isRunning.value) {
    startTime.value = Date.now() - elapsedTime.value;
    timer.value = requestAnimationFrame(updateTime);
    isRunning.value = true;
  } else {
    cancelAnimationFrame(timer.value);
    pauseTime.value = Date.now();
    isRunning.value = false;
  }
};

const resetTimer = () => {
  cancelAnimationFrame(timer.value);
  elapsedTime.value = 0;
  isRunning.value = false;
};

const updateTime = () => {
  currentTime.value = Date.now();
  elapsedTime.value = isRunning.value
    ? currentTime.value - startTime.value
    : elapsedTime.value + currentTime.value - pauseTime.value;
  timer.value = requestAnimationFrame(updateTime);
};
</script>

<style scoped>
.timer-container {
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  width: 225px;
  height: 120px;
  background: #696969;
}
.timer-container_text {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50%;
  width: 100%;
  border-bottom: 1px solid #9e9e9e;
  color: #9e9e9e;
}

button {
  background: transparent;
  border: none;
  cursor: pointer;
}

.timer-container_buttons {
  display: flex;
  gap: 36px;
  align-items: center;
  justify-content: center;
  height: 49%;
}

.active {
  color: white;
  border-color: white;
  fill: white;
  transition: all 0.5s ease;
}
</style>
