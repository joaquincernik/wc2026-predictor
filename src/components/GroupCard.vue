<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  group: Object,
  prediction: String
})

const emit = defineEmits(['predict'])

const selected = ref(props.prediction || '')

watch(() => props.prediction, (newVal) => {
  selected.value = newVal || ''
})

const handleSelect = (option) => {
  selected.value = option
  emit('predict', props.group.name, option)
}

const getTeamFlag = (team) => {
  const flags = {
    'Mexico': '🇲🇽',
    'South Africa': '🇿🇦',
    'South Korea': '🇰🇷',
    'Czechia': '🇨🇿',
    'Canada': '🇨🇦',
    'Bosnia & Herzegovina': '🇧🇦',
    'Qatar': '🇶🇦',
    'Switzerland': '🇨🇭',
    'Brazil': '🇧🇷',
    'Morocco': '🇲🇦',
    'Haiti': '🇭🇹',
    'Scotland': '🏴󠁧󠁢󠁳󠁣󠁴󠁿',
    'USA': '🇺🇸',
    'Paraguay': '🇵🇾',
    'Australia': '🇦🇺',
    'Türkiye': '🇹🇷',
    'Germany': '🇩🇪',
    'Curaçao': '🇨🇼',
    'Netherlands': '🇳🇱',
    'Japan': '🇯🇵',
    'Ivory Coast': '🇨🇮',
    'Ecuador': '🇪🇨',
    'Sweden': '🇸🇪',
    'Tunisia': '🇹🇳',
    'Belgium': '🇧🇪',
    'Egypt': '🇪🇬',
    'Iran': '🇮🇷',
    'New Zealand': '🇳🇿',
    'Spain': '🇪🇸',
    'Cape Verde': '🇨🇻',
    'Saudi Arabia': '🇸🇦',
    'Uruguay': '🇺🇾',
    'France': '🇫🇷',
    'Senegal': '🇸🇳',
    'Iraq': '🇮🇶',
    'Norway': '🇳🇴',
    'Argentina': '🇦🇷',
    'Algeria': '🇩🇿',
    'Austria': '🇦🇹',
    'Jordan': '🇯🇴',
    'Portugal': '🇵🇹',
    'DR Congo': '🇨🇩',
    'Uzbekistan': '🇺🇿',
    'Colombia': '🇨🇴',
    'England': '🏴󠁧󠁢󠁥󠁮󠁧󠁿',
    'Croatia': '🇭🇷',
    'Ghana': '🇬🇭',
    'Panama': '🇵🇦'
  }
  return flags[team] || '⚽'
}
</script>

<template>
  <div class="group-card" :class="{ predicted: prediction }">
    <div class="group-header">
      <h2>Group {{ group.name }}</h2>
    </div>

    <div class="teams-list">
      <div v-for="team in group.teams" :key="team" class="team">
        <span class="flag">{{ getTeamFlag(team) }}</span>
        {{ team }}
      </div>
    </div>

    <div class="match-info">
      <div class="match-label">First Match</div>
      <div class="match-teams">
        <span>{{ group.firstMatch.home }}</span>
        <span class="vs">vs</span>
        <span>{{ group.firstMatch.away }}</span>
      </div>
      <div class="match-details">
        <div>{{ group.firstMatch.date }}</div>
        <div>{{ group.firstMatch.time }}</div>
        <div class="stadium">{{ group.firstMatch.stadium }}</div>
      </div>
    </div>

    <div class="prediction-section">
      <div class="prediction-label">Your prediction:</div>
      <div class="prediction-buttons">
        <button
          v-for="team in [group.firstMatch.home, 'Draw', group.firstMatch.away]"
          :key="team"
          :class="{ active: selected === team }"
          @click="handleSelect(team)"
        >
          {{ team === 'Draw' ? '🤝 Draw' : getTeamFlag(team) + ' ' + team }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.group-card {
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.group-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

.group-card.predicted {
  border: 2px solid #667eea;
}

.group-header {
  text-align: center;
  margin-bottom: 15px;
}

.group-header h2 {
  margin: 0;
  color: #1a1a2e;
  font-size: 1.5rem;
}

.teams-list {
  margin-bottom: 15px;
  padding: 10px;
  background: #f8f9fa;
  border-radius: 8px;
}

.team {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 0;
  font-weight: 500;
}

.flag {
  font-size: 1.2rem;
}

.match-info {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
  text-align: center;
}

.match-label {
  font-size: 0.8rem;
  opacity: 0.9;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.match-teams {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  font-weight: 600;
  font-size: 1.1rem;
}

.vs {
  opacity: 0.8;
  font-size: 0.9rem;
}

.match-details {
  margin-top: 10px;
  font-size: 0.85rem;
  opacity: 0.9;
}

.stadium {
  margin-top: 4px;
  font-size: 0.8rem;
}

.prediction-section {
  text-align: center;
}

.prediction-label {
  font-size: 0.9rem;
  color: #666;
  margin-bottom: 10px;
}

.prediction-buttons {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: center;
}

.prediction-buttons button {
  flex: 1;
  min-width: 80px;
  padding: 10px 8px;
  border: 2px solid #e0e0e0;
  background: white;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.2s;
}

.prediction-buttons button:hover {
  border-color: #667eea;
  background: #f0f0ff;
}

.prediction-buttons button.active {
  border-color: #667eea;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}
</style>