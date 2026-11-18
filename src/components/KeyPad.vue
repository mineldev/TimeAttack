<script setup>
defineProps({
  tecles: Array,
  bloquejat: Boolean,
  premudes: Array
})

const emit = defineEmits(['premuda'])
</script>

<template>
  <div class="keypad">
    <button
      v-for="tecla in tecles"
      :key="tecla"
      class="key-button"
      :class="{ pulsada: premudes.includes(tecla) }"
      :disabled="bloquejat || premudes.includes(tecla)"
      @click="emit('premuda', tecla)"
    >
      {{ tecla }}
    </button>
  </div>
</template>

<style scoped>
.keypad {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 30px 0;
}

.key-button {
  width: 75px;
  height: 75px;
  font-size: 28px;
  font-weight: bold;
  color: white;
  background: #374151;
  border: 2px solid #60a5fa;
  border-radius: 12px;
  cursor: pointer;
  transition: 0.2s;
}

.key-button:hover:not(:disabled) {
  background: #2563eb;
  transform: translateY(-5px);
}

.key-button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.key-button.pulsada {
  background: #16a34a;
  border-color: #4ade80;
  opacity: 1;
}
</style>