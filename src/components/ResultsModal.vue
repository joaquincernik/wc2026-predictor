<script setup>
import { ref, computed, onMounted } from 'vue'
import { groups } from '../data/groups.js'
import { X, Trophy, CheckCircle, XCircle, RefreshCw } from 'lucide-vue-next'

const props = defineProps({
  predictions: Object,
  show: Boolean
})

const emit = defineEmits(['close', 'reset'])

const results = computed(() => {
  return groups.map(group => {
    const prediction = props.predictions[group.name]
    const actual = group.firstMatch.winner
    const isCorrect = prediction === actual
    return {
      group: group.name,
      match: `${group.firstMatch.home} vs ${group.firstMatch.away}`,
      prediction: prediction || 'No prediction',
      actual,
      isCorrect,
      score: `${group.firstMatch.homeScore} - ${group.firstMatch.awayScore}`
    }
  })
})

const correctCount = computed(() => {
  return results.value.filter(r => r.isCorrect).length
})

const showConfetti = computed(() => correctCount.value >= 8)

onMounted(() => {
  if (showConfetti.value) {
    createConfetti()
  }
})

const createConfetti = () => {
  const colors = ['oklch(65% 0.18 260)', 'oklch(75% 0.15 280)', 'oklch(85% 0.12 280)', 'oklch(70% 0.15 45)', 'oklch(90% 0.1 45)']
  for (let i = 0; i < 100; i++) {
    const confetti = document.createElement('div')
    confetti.className = 'fixed w-2.5 h-2.5 rounded-full z-[9999] pointer-events-none'
    confetti.style.left = Math.random() * 100 + 'vw'
    confetti.style.top = '-10px'
    confetti.style.background = colors[Math.floor(Math.random() * colors.length)]
    confetti.style.animation = `fall ${Math.random() * 2 + 2}s linear forwards`
    confetti.style.animationDelay = Math.random() * 2 + 's'
    document.body.appendChild(confetti)
    setTimeout(() => confetti.remove(), 4000)
  }
}

const getMessage = () => {
  if (correctCount.value >= 10) return '🏆 ¡Exacto! Eres un experto!'
  if (correctCount.value >= 8) return '🎉 ¡Excelente trabajo!'
  if (correctCount.value >= 5) return '👍 ¡No está mal!'
  return '😅 ¡Sigue intentando!'
}
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="show" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background: rgba(0, 0, 0, 0.7); backdrop-filter: blur(8px);">
        <div class="relative w-full max-w-2xl max-h-[85vh] overflow-y-auto rounded-2xl border border-primary/20 shadow-2xl animate-[scale-in_0.3s_ease-out]" style="background: linear-gradient(145deg, oklch(15% 0.02 260), oklch(20% 0.02 260));">
          <button
            class="absolute top-4 right-4 p-2 rounded-full bg-card/50 text-muted-foreground hover:text-foreground hover:bg-card transition-all duration-300 hover:rotate-90 z-10"
            @click="emit('close')"
          >
            <X :size="20" />
          </button>

          <div class="p-6 sm:p-8 text-center border-b border-border/20">
            <Trophy :size="48" class="mx-auto mb-3 text-warning" />
            <h2 class="text-2xl sm:text-3xl font-bold text-foreground mb-2">Resultados</h2>
            <p class="text-5xl sm:text-6xl font-extrabold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">
              {{ correctCount }} / 12
            </p>
            <p class="mt-3 text-muted-foreground text-lg">{{ getMessage() }}</p>
          </div>

          <div class="p-4 sm:p-6 space-y-3">
            <TransitionGroup name="list">
              <div
                v-for="result in results"
                :key="result.group"
                class="flex items-center gap-3 sm:gap-4 p-3 sm:p-4 rounded-xl border transition-all duration-300"
                :class="result.isCorrect 
                  ? 'bg-success/10 border-success/30' 
                  : result.prediction !== 'No prediction' 
                    ? 'bg-error/10 border-error/20' 
                    : 'bg-card/30 border-border/20'"
              >
                <div class="flex-shrink-0">
                  <span class="inline-block px-3 py-1 rounded-full text-xs font-bold text-white" style="background: linear-gradient(135deg, oklch(65% 0.18 260), oklch(75% 0.15 280));">
                    Grupo {{ result.group }}
                  </span>
                </div>
                <div class="flex-1 min-w-0">
                  <p class="text-foreground font-medium text-sm truncate">{{ result.match }}</p>
                  <div class="flex flex-wrap gap-x-3 gap-y-1 mt-1 text-xs">
                    <span class="text-muted-foreground">
                      Prediction: <span :class="result.isCorrect ? 'text-success font-semibold' : ''">{{ result.prediction }}</span>
                    </span>
                    <span class="text-muted-foreground">
                      Result: <span class="text-foreground font-medium">{{ result.actual }} ({{ result.score }})</span>
                    </span>
                  </div>
                </div>
                <div class="flex-shrink-0">
                  <CheckCircle v-if="result.isCorrect" :size="22" class="text-success" />
                  <XCircle v-else-if="result.prediction !== 'No prediction'" :size="22" class="text-error" />
                  <span v-else class="text-muted-foreground/30 text-2xl">-</span>
                </div>
              </div>
            </TransitionGroup>
          </div>

          <div class="p-4 sm:p-6 border-t border-border/20">
            <button
              class="w-full inline-flex items-center justify-center gap-2 px-6 py-3 rounded-full font-semibold transition-all duration-300 hover:shadow-lg hover:scale-[1.02]"
              style="background: linear-gradient(135deg, oklch(65% 0.18 260), oklch(75% 0.15 280)); color: white;"
              @click="emit('reset')"
            >
              <RefreshCw :size="18" />
              Nueva Predicción
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from > div,
.modal-leave-to > div {
  transform: scale(0.9);
}

.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

.list-move {
  transition: transform 0.4s ease;
}

@keyframes fall {
  to {
    transform: translateY(100vh) rotate(720deg);
  }
}
</style>