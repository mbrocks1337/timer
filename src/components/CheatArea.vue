<script setup lang="ts">
import { useLocalStorage } from '@vueuse/core'

const beginningInSeconds = useLocalStorage('cheat-time-start', 0);
const multiplier = useLocalStorage('cheat-time-multiplier', 1);
const speedIncreaseDurationInSeconds = useLocalStorage('speed-increase-duration', 0);
</script>

<template>
  <div class="cheat-area">
    <div>
      <label for="beginning">Begin in Sekunden</label>
      <input type="number" id="beginning" v-model.number="beginningInSeconds" />
    </div>
    <div>
      <label for="beginning">Geschwindigkeitsfaktor</label>
      <input type="number" id="beginning" v-model.number="multiplier" />
    </div>
    <div>
      <label for="duration">Dauer der Geschwindigkeitssteigerung (Sekunden)</label>
      <input type="number" id="duration" v-model.number="speedIncreaseDurationInSeconds" />
    </div>
  </div>
</template>

<style>
.cheat-area {
  display: grid;
  gap: 2rem;
}

.cheat-area label {
  display: block;
}

.cheat-area input {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.5rem;
}
</style>