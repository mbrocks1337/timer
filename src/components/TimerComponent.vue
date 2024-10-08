<script setup lang="ts">
import { computed, onMounted, ref, onUnmounted } from 'vue'
import { useLocalStorage } from '@vueuse/core'

const time = ref(0);
const isRunning = ref(false);

const formattedTime = computed(() =>
  new Intl.DateTimeFormat('de-de', {
    minute: 'numeric',
    second: 'numeric',
    hour12: false,
    // @ts-expect-error fractionalSecondDigits is supported
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat
    fractionalSecondDigits: 2
  }).format(time.value)
)

const INTERVAL_IN_MS = 10
const beginningInSeconds = useLocalStorage('cheat-time-start', 3);
const multiplier = useLocalStorage('cheat-time-multiplier', 3);
const speedIncreaseDurationInSeconds = useLocalStorage('speed-increase-duration', 5);

const timeMultiplier = computed(() => {
  const t = time.value;
  const t0 = beginningInSeconds.value * 1000; // Start time in milliseconds
  const t1 = t0 + speedIncreaseDurationInSeconds.value * 1000; // End time in milliseconds
  const targetMultiplier = multiplier.value * 1.1;

  if (t <= t0) {
    return 1;
  } else if (t >= t1) {
    return targetMultiplier;
  } else {
    const progress = (t - t0) / (t1 - t0); // Calculate progress between 0 and 1
    return 1 + progress * (targetMultiplier - 1); // Linear interpolation
  }
});

let interval: number
function startTimer() {
  isRunning.value = true;
  interval = setInterval(() => {
    time.value += INTERVAL_IN_MS * timeMultiplier.value
  }, INTERVAL_IN_MS)
}

function stopTimer() {
  isRunning.value = false;
  clearInterval(interval)
}

onUnmounted(() => {
  clearInterval(interval)
})
</script>
<template>
  <div class="timer">
    <span>{{ formattedTime }}</span>
    <button v-if="isRunning" class="timer-button timer-button--stop" @click="stopTimer">Stop</button>
    <button v-else class="timer-button timer-button--start" @click="startTimer">Start</button>
  </div>
</template>
<style>
.timer {
  font-size: 20vw;
  display: grid;
  justify-items: center;
  gap: 2rem;
}

.timer-button {
  font-size: 3rem;
  color: var(--color);
  border: none;
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;
}

.timer-button--start {
  background-color: #09651e;
}

.timer-button--stop {
  background-color: #d50000;
}
</style>
