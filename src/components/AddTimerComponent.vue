<template>
  <div class="add-container">
    <button @click="addTimer">
      <svg
        width="20"
        height="20"
        viewBox="0 0 20 20"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <rect x="8.5" width="3" height="20" fill="#9E9E9E" />
        <rect
          y="11.5"
          width="3"
          height="20"
          transform="rotate(-90 0 11.5)"
          fill="#9E9E9E"
        />
      </svg>
    </button>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

const emits = defineEmits(['click']);

const addTimer = () => {
  emits('click');
};
</script>

<style scoped>
.add-container {
  display: flex;
  justify-content: center;
  box-sizing: border-box;
  width: 225px;
  height: 120px;
  background: #696969;
}

button {
  background: transparent;
  border: none;
  cursor: pointer;
}
</style>
