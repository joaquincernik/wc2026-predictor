<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  group: Object,
  prediction: String,
  colors: {
    type: Array,
    default: () => ['oklch(65% 0.18 260)', 'oklch(75% 0.15 280)']
  }
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
    'Mexico': '🇲🇽', 'South Africa': '🇿🇦', 'South Korea': '🇰🇷', 'Czechia': '🇨🇿',
    'Canada': '🇨🇦', 'Bosnia & Herzegovina': '🇧🇦', 'Qatar': '🇶🇦', 'Switzerland': '🇨🇭',
    'Brazil': '🇧🇷', 'Morocco': '🇲🇦', 'Haiti': '🇭🇹', 'Scotland': '🏴󠁧󠁢󠁳󠁣󠁴󠁿',
    'USA': '🇺🇸', 'Paraguay': '🇵🇾', 'Australia': '🇦🇺', 'Türkiye': '🇹🇷',
    'Germany': '🇩🇪', 'Curaçao': '🇨🇼', 'Netherlands': '🇳🇱', 'Japan': '🇯🇵',
    'Ivory Coast': '🇨🇮', 'Ecuador': '🇪🇨', 'Sweden': '🇸🇪', 'Tunisia': '🇹🇳',
    'Belgium': '🇧🇪', 'Egypt': '🇪🇬', 'Iran': '🇮🇷', 'New Zealand': '🇳🇿',
    'Spain': '🇪🇸', 'Cape Verde': '🇨🇻', 'Saudi Arabia': '🇸🇦', 'Uruguay': '🇺🇾',
    'France': '🇫🇷', 'Senegal': '🇸🇳', 'Iraq': '🇮🇶', 'Norway': '🇳🇴',
    'Argentina': '🇦🇷', 'Algeria': '🇩🇿', 'Austria': '🇦🇹', 'Jordan': '🇯🇴',
    'Portugal': '🇵🇹', 'DR Congo': '🇨🇩', 'Uzbekistan': '🇺🇿', 'Colombia': '🇨🇴',
    'England': '🏴󠁧󠁢󠁥󠁮󠁧󠁿', 'Croatia': '🇭🇷', 'Ghana': '🇬🇭', 'Panama': '🇵🇦'
  }
  return flags[team] || '⚽'
}

const gradientBg = `linear-gradient(135deg, ${props.colors[0]}15, ${props.colors[1]}15)`
const gradientBorder = `linear-gradient(135deg, ${props.colors[0]}, ${props.colors[1]})`
const badgeGradient = `linear-gradient(135deg, ${props.colors[0]}, ${props.colors[1]})`
const matchGradient = `linear-gradient(135deg, oklch(85% 0.12 280), oklch(75% 0.15 280))`
const buttonGradient = `linear-gradient(135deg, ${props.colors[0]}, ${props.colors[1]})`

const getButtonClass = (option) => {
  const base = 'flex-1 flex items-center justify-center gap-1 py-2.5 px-1 rounded-xl border-2 transition-all duration-300 hover:-translate-y-0.5 text-xs font-medium min-w-0'
  const baseClasses = 'border-border/20 bg-card/30 text-muted-foreground hover:border-primary/50 hover:bg-card/50'
  const activeClasses = 'text-foreground border-transparent'
  
  if (selected.value === option) {
    return `${base} ${activeClasses} ${getOptionGradient(option)}`
  }
  return `${base} ${baseClasses}`
}

const getOptionGradient = (option) => {
  if (option === 'Draw') return 'bg-warning text-foreground'
  if (option === props.group.firstMatch.home) return 'bg-success text-foreground'
  return 'bg-error text-foreground'
}
</script>

<template>
  <div
    class="relative overflow-hidden rounded-2xl border border-border/30 backdrop-blur-xl transition-all duration-400 hover:-translate-y-2 hover:shadow-2xl hover:shadow-primary/10 group"
    :style="{ background: gradientBg }"
  >
    <div class="absolute inset-0 -z-10 opacity-0 group-hover:opacity-100 transition-opacity duration-500"
      :style="{ background: `radial-gradient(circle at 50% 0%, ${colors[0]}30, transparent 70%)` }">
    </div>

    <div class="p-5">
      <div class="mb-4">
        <span
          class="inline-block px-4 py-1.5 rounded-full text-sm font-bold text-white uppercase tracking-wide"
          :style="{ background: badgeGradient }"
        >
          Group {{ group.name }}
        </span>
      </div>

      <div class="bg-card/40 rounded-xl p-3 mb-4">
        <div v-for="team in group.teams" :key="team" class="flex items-center gap-2 py-2 border-b border-border/10 last:border-0">
          <span class="text-lg flex-shrink-0">{{ getTeamFlag(team) }}</span>
          <span class="text-foreground font-medium text-xs truncate">{{ team }}</span>
        </div>
      </div>

      <div class="rounded-xl p-4 mb-4" :style="{ background: matchGradient }">
        <div class="flex items-center justify-between gap-2 mb-3">
          <div class="flex items-center gap-1.5 text-foreground font-semibold text-xs min-w-0">
            <span class="text-base flex-shrink-0">{{ getTeamFlag(group.firstMatch.home) }}</span>
            <span class="truncate">{{ group.firstMatch.home }}</span>
          </div>
          <span class="px-2 py-0.5 rounded-full text-xs font-bold text-white flex-shrink-0" style="background: linear-gradient(135deg, oklch(65% 0.18 260), oklch(75% 0.15 280));">
            VS
          </span>
          <div class="flex items-center gap-1.5 text-foreground font-semibold text-xs min-w-0">
            <span class="truncate">{{ group.firstMatch.away }}</span>
            <span class="text-base flex-shrink-0">{{ getTeamFlag(group.firstMatch.away) }}</span>
          </div>
        </div>
        <div class="flex flex-col gap-1 text-xs text-foreground/80">
          <div class="flex items-center gap-1.5">
            <span>📅</span> {{ group.firstMatch.date }}
          </div>
          <div class="flex items-center gap-1.5">
            <span>🕐</span> {{ group.firstMatch.time }}
          </div>
          <div class="truncate text-foreground/60">
            <span>📍</span> {{ group.firstMatch.stadium }}
          </div>
        </div>
      </div>

      <div>
        <div class="flex items-center justify-between mb-3 text-sm">
          <span class="text-muted-foreground">Your prediction</span>
          <span v-if="prediction" class="text-success font-bold">✓</span>
        </div>
        <div class="flex gap-1.5">
          <button
            v-for="option in [group.firstMatch.home, 'Draw', group.firstMatch.away]"
            :key="option"
            :class="getButtonClass(option)"
            @click="handleSelect(option)"
            :title="option"
          >
            <span v-if="option === 'Draw'" class="text-lg">🤝</span>
            <span v-else class="text-xl">{{ getTeamFlag(option) }}</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>