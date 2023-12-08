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
let quizSocket = null;

const initializeWebSocket = () => {
  console.log('WebSocket initializing...');
  quizSocket = new WebSocket('ws://localhost:3000/primus');
  
  quizSocket.onmessage = handleWebSocketMessage;
};

const handleWebSocketMessage = (event) => {
  console.log('WebSocket message received:', event.data);
  const newData = JSON.parse(event.data);
  
  if (newData.action === 'updateStats') {
    updateTeamStats(newData.team, newData.score);
  }
};

const updateTeamStats = (teamName, newScore) => {
  const teamToUpdate = teams.value.find((team) => team.name === teamName);
  
  if (teamToUpdate) {
    teamToUpdate.score = newScore;
    console.log('Team stats updated:', teamToUpdate);
  }
};

const onMountedHandler = () => {
  console.log('Component mounted');
  initializeWebSocket();
};

const updateStatistics = () => {
  const message = {
    action: 'updateStats',
    team: selectedTeam.value,
    score: score.value,
  };

  // Send message to WebSocket server
  if (quizSocket && quizSocket.readyState === WebSocket.OPEN) {
    quizSocket.send(JSON.stringify(message));
    console.log('Message sent:', message);
  }

  // Reset input values
  selectedTeam.value = '';
  score.value = 0;
};

onMounted(onMountedHandler);
