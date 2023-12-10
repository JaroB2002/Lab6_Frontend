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
      const existingRider = data.value.find(item => item.rider === message.rider);
      if (existingRider) {
        // If the rider already exists, update the score
        existingRider.score = message.score;
      } else {
        // If the rider doesn't exist, add a new entry
        data.value.push({
          "rider": message.rider,
          "score": message.score
        });
      }
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
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f8f9fa;
    border: 2px solid #e9ecef;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  }
  
  .scoredisplay-title {
    font-size: 2.5rem;
    text-align: center;
    color: #343a40;
    margin-bottom: 20px;
    text-transform: uppercase;
    font-family: 'Arial', sans-serif;
    letter-spacing: 2px;
  }
  
  .scoredisplay-list {
    list-style-type: none;
    padding: 0;
  }
  
  .scoredisplay-item {
    margin-bottom: 15px;
    border: 1px solid #d6d8db;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #fff;
    border-radius: 10px;
    transition: all 0.3s ease-in-out;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  .scoredisplay-item:hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }
  
  .rider-name {
    font-size: 1.3rem;
    color: #343a40;
    font-weight: bold;
  }
  
  .rider-score {
    color: #007bff;
    font-size: 1.5rem;
    font-weight: bold;
  }
  
  .team-name {
    font-weight: bold;
    margin-right: 8px;
    color: #343a40;
  }
  
  .team-score {
    color: #28a745;
    font-size: 1.3rem;
    font-weight: bold;
  }

  /* Voeg een subtiele animatie toe aan de scoreboard items */
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .scoredisplay-list li {
    animation: fadeInUp 0.5s ease-out;
  }
</style>
