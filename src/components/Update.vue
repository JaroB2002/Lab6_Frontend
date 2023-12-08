<script setup>
import { ref, onMounted } from 'vue';

const riders = ref([
  "Wout Van Aert", "Mathieu Van Der Poel", "Tom Pidcock", "Eli Iserbyt",
  "Laurens Sweeck", "Michael Vanthourenhout", "Toon Aerts", "Quinten Hermans",
  "Lars Van Der Haar", "Corné Van Kessel"
]);
const selectedTeam = ref(null);
const score = ref(null);

let socket = null;

onMounted(() => {
  socket = new WebSocket('wss://lab6-backend-3m8h.onrender.com/primus');
});

const updateStats = () => {
  const message = {
    "action": "updateStats",
    "rider": selectedTeam.value,
    "score": score.value
  };0
  socket.send(JSON.stringify(message));
};
</script>

<template>
  <div class="update-stats-container">
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

<style scoped>
/* Stijlen voor de volledige component */
.update-stats-container {
  max-width: 400px;
  margin: 0 auto;
}

h1 {
  color: #3498db;
  font-size: 28px;
  margin-bottom: 20px;
}

label {
  display: block;
  margin-top: 15px;
  color: #3498db;
  font-weight: bold;
}

select, input {
  margin: 10px 0;
  padding: 10px;
  width: 100%;
  border: 1px solid #bdc3c7;
  border-radius: 5px;
}

button {
  background-color: #2ecc71;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #27ae60;
}
</style>
