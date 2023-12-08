<script setup>
import { ref } from 'vue';

const riders = ref(["Wout Van Aert", "Mathieu Van Der Poel", "Tom Pidcock"]);
const selectedTeam = ref(null);
const score = ref(null);

const updateStats = () => {
  // Implement your logic for updating stats here (since there's no backend or socket)
  console.log("Updating stats:", selectedTeam.value, score.value);
};
</script>

<template>
  <div>
    <h1>Update Statistics</h1>
    <label for="teamSelect">Select Rider:</label>
    <select v-model="selectedTeam" id="teamSelect">
      <option v-for="rider in riders" :key="rider" :value="rider">{{ rider }}</option>
    </select>
    <label for="scoreInput">Enter Score:</label>
    <input type="text" v-model="score" id="scoreInput">
    <button @click="updateStats">Update</button>
  </div>
</template>
