<script setup>
// Hier kunnen eventuele imports en andere configuraties komen
</script>

<template>
  <div class="app-container">
    <router-link to="/" class="nav-link">Display score</router-link>
    <router-link to="/updatestats" class="nav-link">Update Stats</router-link>
    <router-view></router-view>
  </div>
</template>

<style scoped>
/* Voeg stijlen toe aan je component */

.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.nav-link {
  margin: 10px;
  padding: 10px;
  text-decoration: none;
  color: #3498db;
  font-weight: bold;
  border: 1px solid #3498db;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.nav-link:hover {
  background-color: #3498db;
  color: white;
}
</style>
