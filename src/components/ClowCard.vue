<script setup lang="ts">
import { ref } from 'vue';
import gsap from 'gsap';
import type { ClowCard } from '../types/cards';

const props = defineProps<{
  carta: ClowCard;
  disabled?: boolean;
}>();

const emit = defineEmits<{
  (e: 'cardRevealed', carta: ClowCard): void;
}>();

const cardRef = ref<HTMLElement | null>(null);
const revelada = ref(false);

const revelarCarta = () => {
  if (props.disabled || revelada.value) return;
  
  const card = cardRef.value;
  if (!card) return;

  gsap.to(card, {
    rotationY: 180,
    duration: 0.8,
    ease: "power2.inOut",
    onComplete: () => {
      revelada.value = true;
      emit('cardRevealed', props.carta);
    }
  });

  // Sonido al girar
  const audio = new Audio('/sounds/release.mp3');
  audio.play();
};
</script>

<template>
  <div 
    ref="cardRef"
    class="clow-card" 
    :class="{ 'disabled': disabled }"
    @click="revelarCarta"
  >
    <div class="card-inner" :class="{ 'is-flipped': revelada }">
      <!-- Reverso de la carta -->
      <div class="card-face card-back">
        <div class="card-back-design"></div>
      </div>

      <!-- Parte frontal de la carta -->
      <div class="card-face card-front">
        <img :src="carta.imagen" :alt="carta.nombre" class="card-image">
        <div class="card-content">
          <h3>{{ carta.nombre }}</h3>
          <p>{{ carta.significado }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.clow-card {
  width: 200px;
  height: 350px; /* Aumenta la altura */
  perspective: 1000px;
  cursor: pointer;
  margin: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* Distribuye el contenido */
}


.disabled {
  cursor: not-allowed;
  opacity: 0.7;
}

/* 🔄 Corrección en la rotación inicial */
.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.8s ease-in-out;
  transform: rotateY(0deg); /* 🔄 Asegura que inicie sin girar */
}

.is-flipped {
  transform: rotateY(180deg);
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 15px;
  box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
  overflow: hidden;
}

/* 🎴 Estilo para el reverso de la carta */
.card-back {
  background: linear-gradient(45deg, #2c0a4a, #4a1073);
  border: 2px solid #ffd700;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 🎴 Estilo para el frente de la carta */
.card-front {
  transform: rotateY(180deg);
  background: white;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%; /* Asegura que ocupe toda la carta */
}

.card-back-design {
  width: 100%;
  height: 100%;
  background-image: url('https://raw.githubusercontent.com/stackblitz/content/main/cards/back.jpg');
  background-size: cover;
  background-position: center;
  opacity: 0.8;
}

.grid-cartas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); /* Organiza en filas */
  gap: 1rem; /* Espaciado entre cartas */
  justify-content: center;
  max-width: 900px;
  margin: 0 auto;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.card-content {
  margin-top: auto; /* Empuja el texto hacia abajo */
  text-align: center;
  padding: 0.5rem;
}

.card-content h3 {
  font-size: 0.9rem;
  color: #4a1073;
  margin-bottom: 0.5rem;
}

.card-content p {
  font-size: 0.7rem;
  color: #666;
}
</style>