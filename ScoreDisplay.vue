<template>
    <div>
      <h1>Live Scoreboard</h1>
      <ul>
        <li v-for="item in data" :key="item.rider">
          <span>{{ item.rider }}</span>
          <span>{{ item.score }}</span>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const data = ref([
    { "rider": "Wout Van Aert", "score": 31 },
    { "rider": "Mathieu Van Der Poel", "score": 1 },
    { "rider": "Eli Iserbyt", "score": 10 },
    { "rider": "Laurens Sweeck", "score": 8 },
  ]);
  </script>
  