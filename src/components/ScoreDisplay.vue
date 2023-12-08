<template>
    <div class="scoredisplay-container">
      <h1 class="scoredisplay-title">Live Scoreboard</h1>
      <ul class="scoredisplay-list">
        <li v-for="item in sortedData" :key="item.rider" class="scoredisplay-item">
          <span class="rider-name">{{ item.rider }}</span>
          <span class="rider-score">{{ item.score }}</span>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const data = ref([
    { "rider": "Wout Van Aert", "score": 31 },
    { "rider": "Mathieu Van Der Poel", "score": 1 },
    { "rider": "Eli Iserbyt", "score": 10 },
    { "rider": "Laurens Sweeck", "score": 8 },
  ]);
  
  let socket = null;
  
  onMounted(() => {
    socket = new WebSocket('wss://lab6-backend-3m8h.onrender.com/primus');
    // Luister naar nieuwe gegevens
    socket.onmessage = function (event) {
      const message = JSON.parse(event.data);
      console.log(message);
      if (message.action === 'updateStats') {
        data.value.push({
          "rider": message.rider,
          "score": message.score
        });
        sortData();
      }
    };
  });
  
  const sortedData = ref([]);
  
  // Sorteer de data wanneer deze wordt bijgewerkt
  onMounted(() => {
    sortData();
  });
  
  function sortData() {
    sortedData.value = [...data.value].sort((a, b) => b.score - a.score);
  }
  </script>
  
  <style scoped>
  /* Stijlen voor de volledige component */
  .scoredisplay-container {
    max-width: 600px;
    margin: 0 auto;
  }
  
  .scoredisplay-title {
    font-size: 2rem;
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .scoredisplay-list {
    list-style-type: none;
    padding: 0;
  }
  
  .scoredisplay-item {
    margin-bottom: 10px;
    border: 1px solid #ccc;
    padding: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #f9f9f9;
    border-radius: 8px;
    transition: background-color 0.3s ease-in-out;
  }
  
  .scoredisplay-item:hover {
    background-color: #e0e0e0;
  }
  
  .team-name {
    font-weight: bold;
    margin-right: 8px;
  }
  
  .team-score {
    color: #007bff;
    font-size: 1.2rem;
  }
  </style>
  