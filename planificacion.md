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