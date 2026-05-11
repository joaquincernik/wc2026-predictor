# Plan: World Cup 2026 Predictor

## 1. Tech Stack

- **Framework**: Vue 3 (Composition API)
- **Build**: Vite
- **Lenguaje**: JavaScript/TypeScript
- **Hosting**: GitHub Pages (static build)
- **Sin backend**: Datos embebidos o API gratuita

## 2. Estructura del Proyecto

```
wc2026-predictor/
├── index.html
├── package.json
├── vite.config.js
├── src/
│   ├── main.js
│   ├── App.vue
│   ├── components/
│   │   ├── GroupCard.vue
│   │   └── MatchPredictor.vue
│   ├── data/
│   │   └── groups.js (datos hardcodeados de los 12 grupos)
│   └── style.css
└── README.md
```

## 3. Datos de los Primeros Partidos (48 equipos, 12 grupos)

**Grupo A**: Mexico, South Africa, South Korea, Czechia
**Grupo B**: Canada, Bosnia & Herzegovina, Qatar, Switzerland
**Grupo C**: Brazil, Morocco, Haiti, Scotland
**Grupo D**: USA, Paraguay, Australia, Türkiye
**Grupo E**: Germany, Curaçao, Netherlands, Japan
**Grupo F**: Ivory Coast, Ecuador, Sweden, Tunisia
**Grupo G**: Belgium, Egypt, Iran, New Zealand
**Grupo H**: Spain, Cape Verde, Saudi Arabia, Uruguay
**Grupo I**: France, Senegal, Iraq, Norway
**Grupo J**: Argentina, Algeria, Austria, Jordan
**Grupo K**: Portugal, DR Congo, Uzbekistan, Colombia
**Grupo L**: England, Croatia, Ghana, Panama

Primeros partidos (jornada 1):
- A: Mexico vs South Africa (Jun 11)
- B: Canada vs Bosnia & Herzegovina (Jun 12)
- C: Brazil vs Morocco (Jun 13)
- D: USA vs Paraguay (Jun 12)
- E: Germany vs Curaçao (Jun 14)
- F: Ivory Coast vs Ecuador (Jun 14)
- G: Belgium vs Egypt (Jun 15)
- H: Spain vs Cape Verde (Jun 15)
- I: France vs Senegal (Jun 16)
- J: Argentina vs Algeria (Jun 16)
- K: Portugal vs DR Congo (Jun 16)
- L: England vs Croatia (Jun 16)

## 4. Funcionalidades

1. **Vista de Grupos**: Mostrar los 12 grupos con sus equipos
2. **Partidos de Jornada 1**: Listar primer partido de cada grupo
3. **Sistema de Predicción**:
   - Seleccionar equipo ganador o "Empate"
   - Guardar predicciones en localStorage
   - Mostrar resumen de predicciones realizadas

## 5. Commands

```bash
# Development
npm install
npm run dev

# Build for GitHub Pages
npm run build

# Preview build
npm run preview
```

## 6. GitHub Pages Config

- Repository: GitHub
- Branch: `main` o `gh-pages`
- Build output: `/dist`
- Homepage: `https://[user].github.io/[repo]/`

## 7. Decisiones de Implementación

- **Datos**: Hardcodear los primeros partidos (evita dependencias de APIs)
- **Estado**: Vue reactivity + localStorage para persistir predicciones
- **Estilo**: CSS simple, diseño responsive, tema deportivo

## 8. Pasos de Ejecución

1. Inicializar proyecto Vue + Vite
2. Crear datos de grupos y partidos
3. Componente GroupCard para mostrar equipos por grupo
4. Componente MatchPredictor para seleccionar resultado
5. Implementar lógica de guardado en localStorage
6. Configurar vite.config.js para GitHub Pages
7. Build y deploy

## 9. Fase 2: Simulación y Resultados

### 9.1 Datos de Resultados Reales

Los partidos ya tienen fecha y resultado real (en vivo o Hardcoded):
- A: Mexico vs South Africa - **2-1** (Mexico)
- B: Canada vs Bosnia & Herzegovina - **0-0** (Draw)
- C: Brazil vs Morocco - **2-1** (Brazil)
- D: USA vs Paraguay - **0-0** (Draw)
- E: Germany vs Curaçao - **4-0** (Germany)
- F: Ivory Coast vs Ecuador - **2-1** (Ecuador)
- G: Belgium vs Egypt - **0-0** (Draw)
- H: Spain vs Cape Verde - **2-1** (Spain)
- I: France vs Senegal - **1-0** (France)
- J: Argentina vs Algeria - **2-0** (Argentina)
- K: Portugal vs DR Congo - **2-0** (Portugal)
- L: England vs Croatia - **1-1** (Draw)

*NOTA: Resultados hardcodeados como ejemplo. Se pueden actualizar con datos reales.*

### 9.2 Funcionalidad de Simulación

1. **Botón "Simular Resultados"**: Genera los resultados de cada partido
2. **Comparación automática**: Compara predicción del usuario vs resultado real
3. **Contador de aciertos**: Muestra X/12 aciertos
4. **Detalle por grupo**: Muestra ✓ o ✗ para cada grupo
5. **Panel de resumen**: Modal o sección con estadísticas finales

### 9.3 Estructura de Datos

```javascript
// En groups.js - agregar campo de resultado real
{
  name: 'A',
  teams: [...],
  firstMatch: {
    home: 'Mexico',
    away: 'South Africa',
    date: 'June 11, 2026',
    // Nuevo:
    result: { home: 2, away: 1 },
    winner: 'Mexico' // o 'Draw'
  }
}
```

### 9.4 Componente ResultsModal.vue

- Modal overlay con los resultados
- Lista de grupos con predicción vs resultado
- Número total de aciertos
- Animación de celebración si tiene > 8 aciertos

## 10. Mejoras de Diseño (Frontend Design)

### 10.1 Bibliotecas a instalar

- **@vueuse/core** - Utilidades de Vue (localStorage reactivo)
- **animate.css** - Animaciones CSS
- **lucide-vue-next** - Iconos modernos

### 10.2 Mejoras visuales

1. **Tarjetas de grupos**:
   - Gradientes según grupo (A-L con colores distintos)
   - Efectos glassmorphism
   - Hover con lift animation

2. **Header mejorado**:
   - Fondo con patrón de fútbol o textura
   - Título con texto gradiente
   - Contador de predicciones animado

3. **Sección de predicción**:
   - Botones con estados (hover, active, selected)
   - Transiciones suaves
   - Iconos de selección visual

4. **Modal de resultados**:
   - Backdrop blur
   - Animación de entrada (scale + fade)
   - Confetti si > 8 aciertos

5. ** Responsive**:
   - Grid adaptativo
   - Tarjetas apiladas en móvil

### 10.3 Paleta de colores

- Primary: `#667eea` → `#764ba2` (gradiente Violeta)
- Secondary: `#f093fb` → `#f5576c` (gradiente Rosa)
- Background: Degradado global
- Texto: `#1a1a2e` (oscuro) / `#ffffff` (claro)

### 10.4 Animaciones

- Entrada de tarjetas: `fade-in-up` con stagger
- Hover en cards: `transform: translateY(-4px)`
- Selección: `scale(0.95)` → `scale(1)`
- Modal: `fade-in` + `zoom-in`

## 11. Comandos de Desarrollo

```bash
# Desarrollo
npm run dev

# Build producción
npm run build

# Preview local
npm run preview
```

## 12. Deploy a GitHub Pages

1. Hacer commit y push a main
2. En GitHub: Settings → Pages → Deploy from branch: main, folder: /dist
3. URL final: `https://[usuario].github.io/wc2026-predictor/`