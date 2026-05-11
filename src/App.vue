<script setup>
import { ref, onMounted } from 'vue'
import { groups } from './data/groups.js'
import GroupCard from './components/GroupCard.vue'

const predictions = ref({})

const loadPredictions = () => {
  const saved = localStorage.getItem('wc2026-predictions')
  if (saved) {
    predictions.value = JSON.parse(saved)
  }
}

const savePrediction = (groupName, prediction) => {
  predictions.value[groupName] = prediction
  localStorage.setItem('wc2026-predictions', JSON.stringify(predictions.value))
}

const predictedCount = ref(0)

onMounted(() => {
  loadPredictions()
  predictedCount.value = Object.keys(predictions.value).length
})
</script>

<template>
  <div class="container">
    <header>
      <h1>🏆 World Cup 2026</h1>
      <p class="subtitle">Predict the first match of each group</p>
      <div class="stats">
        Predictions: {{ predictedCount }} / 12
      </div>
    </header>

    <main>
      <div class="groups-grid">
        <GroupCard
          v-for="group in groups"
          :key="group.name"
          :group="group"
          :prediction="predictions[group.name]"
          @predict="savePrediction"
        />
      </div>
    </main>

    <footer>
      <p>FIFA World Cup 2026 - Canada, Mexico, USA</p>
    </footer>
  </div>
</template>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

header {
  text-align: center;
  margin-bottom: 30px;
}

header h1 {
  font-size: 2.5rem;
  color: #1a1a2e;
  margin: 0;
}

.subtitle {
  color: #666;
  margin: 10px 0;
}

.stats {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  display: inline-block;
  padding: 8px 20px;
  border-radius: 20px;
  font-weight: 600;
}

.groups-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

footer {
  text-align: center;
  margin-top: 40px;
  color: #888;
  font-size: 0.9rem;
}
</style>