<script setup lang="ts">
import TimerComponent from '@/components/TimerComponent.vue'
import { ref } from 'vue'
import CheatArea from '@/components/CheatArea.vue'

const isCheatAreaVisible = ref(false);
</script>

<template>
  <div>
    <div class="cheat-area-button" @click="isCheatAreaVisible = !isCheatAreaVisible">&nbsp;</div>
    <CheatArea v-if="isCheatAreaVisible" />
    <TimerComponent v-else />
  </div>
</template>

<style>
:root {
  --background-color: #181818;
  --color: #d5d5d5;
}

body {
  background-color: var(--background-color);
  color: var(--color);
  font-family: system-ui, sans-serif;
  display: grid;
  place-items: center;
  min-height: 100vh;
  margin: 0;
}

.cheat-area-button {
  position: fixed;
  top: 12px;
  right: 12px;
}
</style>
