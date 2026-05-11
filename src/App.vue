<script setup>
import { ref, computed } from 'vue'
import { groups, groupColors } from './data/groups.js'
import GroupCard from './components/GroupCard.vue'
import ResultsModal from './components/ResultsModal.vue'
import { Play, RotateCcw } from 'lucide-vue-next'
import { useStorage } from '@vueuse/core'

const predictions = useStorage('wc2026-predictions', {})

const showResults = ref(false)

const predictedCount = computed(() => Object.keys(predictions.value).length)
const canSimulate = computed(() => predictedCount.value > 0)

const openResults = () => {
  showResults.value = true
}

const resetPredictions = () => {
  predictions.value = {}
  showResults.value = false
}

const getGroupColor = (groupName) => {
  return groupColors[groupName] || ['#667eea', '#764ba2']
}
</script>

<template>
  <div class="min-h-screen relative overflow-hidden">
    <div class="fixed inset-0 -z-10"
      style="background: radial-gradient(circle at 20% 80%, oklch(65% 0.18 260 / 0.15) 0%, transparent 50%), radial-gradient(circle at 80% 20%, oklch(75% 0.15 280 / 0.15) 0%, transparent 50%), radial-gradient(circle at 50% 50%, oklch(85% 0.12 280 / 0.1) 0%, transparent 70%);">
    </div>

    <header class="relative border-b border-border/30 backdrop-blur-xl bg-card/50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8">
        <div class="flex flex-col sm:flex-row items-center justify-between gap-6">
          <div class="text-center sm:text-left">
            <h1 class="text-3xl sm:text-4xl lg:text-5xl font-extrabold tracking-tight" style="background: linear-gradient(135deg, oklch(65% 0.18 260), oklch(85% 0.12 280), oklch(70% 0.15 45)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;">
              <span class="mr-2">🏆</span>
              World Cup 2026
            </h1>
            <p class="mt-2 text-muted-foreground text-base sm:text-lg">
              Predict the first match of each group
            </p>
          </div>

          <div class="flex items-center gap-4 bg-card/50 backdrop-blur-sm rounded-xl p-4 border border-border/30">
            <div class="text-center px-3">
              <span class="block text-2xl sm:text-3xl font-bold text-primary">{{ predictedCount }}</span>
              <span class="text-xs text-muted-foreground uppercase tracking-wider">Predictions</span>
            </div>
            <div class="w-px h-10 bg-border/50"></div>
            <div class="text-center px-3">
              <span class="block text-2xl sm:text-3xl font-bold text-secondary">12</span>
              <span class="text-xs text-muted-foreground uppercase tracking-wider">Groups</span>
            </div>
          </div>
        </div>

        <div v-if="predictedCount > 0" class="mt-6 flex justify-center gap-3">
          <button
            class="inline-flex items-center gap-2 px-6 py-3 rounded-full font-semibold text-sm transition-all duration-300 hover:shadow-lg hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed"
            style="background: linear-gradient(135deg, oklch(65% 0.18 260), oklch(75% 0.15 280)); color: white;"
            @click="openResults"
            :disabled="!canSimulate"
          >
            <Play :size="18" />
            Simulate Results
          </button>
          <button
            class="p-3 rounded-full border border-border/30 bg-card/30 text-muted-foreground hover:text-foreground hover:bg-card/50 transition-all duration-300 hover:rotate-180"
            @click="resetPredictions"
          >
            <RotateCcw :size="18" />
          </button>
        </div>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
        <GroupCard
          v-for="(group, index) in groups"
          :key="group.name"
          :group="group"
          :prediction="predictions[group.name]"
          :colors="getGroupColor(group.name)"
          :style="{ animationDelay: `${index * 0.08}s` }"
          class="animate-[fade-up_0.5s_ease-out_backwards]"
          @predict="(name, pred) => predictions[name] = pred"
        />
      </div>
    </main>

    <footer class="text-center py-8 text-muted-foreground text-sm border-t border-border/20">
      <p> FIFA World Cup 2026 - Canada, Mexico, USA </p>
      <p class="mt-1 text-muted-foreground/60">Make your predictions before the tournament starts!</p>
    </footer>

    <ResultsModal
      :predictions="predictions"
      :show="showResults"
      @close="showResults = false"
      @reset="resetPredictions"
    />
  </div>
</template>