<script setup lang="ts">
import { ref, computed } from 'vue';
import ClowCard from './components/ClowCard.vue';
import { CARTAS_CLOW } from './data/cartas-clow';
import type { ClowCard as ClowCardType } from './types/cards';

const cartasSeleccionadas = ref<ClowCardType[]>([]);
const cartasDisponibles = computed(() => 
  CARTAS_CLOW.sort(() => Math.random() - 0.5).slice(0, 5)
);

const handleCardRevealed = (carta: ClowCardType) => {
  cartasSeleccionadas.value.push(carta);
};
</script>

<template>
  <div class="min-h-screen bg-gradient-to-b from-purple-900 to-indigo-900 py-8 px-4">
    <div class="max-w-4xl mx-auto">
      <header class="text-center mb-12">
        <h1 class="titulo-clow">🔮 Tirada Mágica de Cartas Clow</h1>
        <p class="subtitulo-clow">Selecciona una carta para revelar su mensaje mágico...</p>
      </header>

      <!-- Contenedor de cartas en filas -->
      <div class="grid-cartas">
        <ClowCard
          v-for="carta in cartasDisponibles"
          :key="carta.nombre"
          :carta="carta"
          :disabled="cartasSeleccionadas.length >= 3"
          @card-revealed="handleCardRevealed"
        />
      </div>

      <div v-if="cartasSeleccionadas.length > 0" class="mt-12 text-center">
        <h2 class="text-2xl font-bold text-yellow-400 mb-4">Tu Lectura Mágica</h2>
        <div class="bg-white/10 rounded-lg p-6 backdrop-blur-sm">
          <div v-for="(carta, index) in cartasSeleccionadas" :key="carta.nombre" class="mb-4">
            <h3 class="text-xl text-yellow-300">{{ carta.nombre }}</h3>
            <p class="text-white">{{ carta.significado }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<style>
@import url('https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@400;700&display=swap');

:root {
  font-family: 'Cinzel Decorative', cursive;
}

body {
  margin: 0;
  min-height: 100vh;
  background: #1a1a1a;
}
.grid-cartas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); /* Se acomodan en filas */
  gap: 1rem; /* Espaciado entre cartas */
  justify-content: center;
  width: 100%;
  max-width: 900px; /* O el ancho que desees */
  margin: 0 auto; /* Para centrar */
}

</style>