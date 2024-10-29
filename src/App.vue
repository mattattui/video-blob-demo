<script setup>
import { ref } from "vue";
import BlobAnimation from "./components/BlobAnimation.vue";

const currentState = ref("dormant");
</script>

<template>
  <BlobAnimation :state="currentState" />
  <div class="controls">
    <button @click="currentState = 'dormant'">Dormant</button>
    <button @click="currentState = 'listening'">Listening</button>
    <button @click="currentState = 'speaking'">Speaking</button>
  </div>
</template>

<style scoped>
.controls {
  text-align: center;
  margin-top: 20px;
}

button {
  margin: 0 5px;
  padding: 8px 16px;
  cursor: pointer;
  background: #333;
  border: 1px solid #666;
  color: white;
  border-radius: 4px;
}

button:hover {
  background: #444;
}
</style>
