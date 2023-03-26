<template>
  <div class="container">
    <timer-component v-for="_ in numberOfCounts"></timer-component>
    <add-timer-component @click="addTimer"></add-timer-component>
  </div>
</template>

<script setup>
import TimerComponent from './components/TimerComponent.vue';
import AddTimerComponent from './components/AddTimerComponent.vue';

import { ref } from 'vue';

const numberOfCounts = ref(new Array().length);

function addTimer() {
  numberOfCounts.value++;
}
</script>

<style scoped>
.container {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 50px;
  align-items: center;
  justify-content: center;
}

@media screen and (min-width: 768px) {
  .container {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media screen and (min-width: 1024px) {
  .container {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
