<template>
  <div class="stage">
    <!-- Ambient orbs -->
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>
    <div class="orb orb-3"></div>

    <!-- Floating petals -->
    <div class="petals" aria-hidden="true">
      <span v-for="p in petals" :key="p.id" class="petal" :style="p.style">{{ p.emoji }}</span>
    </div>

    <!-- Grain overlay -->
    <div class="grain"></div>

    <!-- Card -->
    <main class="card">
      <div class="card-inner">
        <Transition name="step" mode="out-in">
          <component :is="currentStep.component" :key="step" @next="nextStep" />
        </Transition>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import WelcomeStep from './components/WelcomeStep.vue'
import QuizStep from './components/QuizStep.vue'
import VideoStep from './components/VideoStep.vue'

const step = ref(0)
const steps = [
  { component: WelcomeStep },
  { component: QuizStep },
  { component: VideoStep },
]
const currentStep = computed(() => steps[step.value] || steps[0])
const nextStep = () => { if (step.value < steps.length - 1) step.value++ }

const petals = Array.from({ length: 14 }, (_, i) => ({
  id: i,
  emoji: ['🌸', '✨', '🌺', '💕', '🌷', '⭐'][i % 6],
  style: {
    left: `${(i * 7.3 + 3) % 100}%`,
    animationDuration: `${12 + (i * 1.7) % 10}s`,
    animationDelay: `${(i * 1.3) % 8}s`,
    fontSize: `${12 + (i % 5) * 4}px`,
    opacity: 0.4 + (i % 4) * 0.12,
  }
}))
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --wine: #6B1A35;
  --rose: #C0476A;
  --blush: #F2B8C6;
  --petal: #FDE8EF;
  --gold: #D4A853;
  --cream: #FEF8F2;
  --deep: #2A0A16;
  --text: #3D1020;
  --muted: #8B5A6A;
  --serif: 'Cormorant Garamond', Georgia, serif;
  --sans: 'DM Sans', system-ui, sans-serif;
  --card-w: min(480px, 94vw);
}

body { font-family: var(--sans); }

.stage {
  min-height: 100dvh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: radial-gradient(ellipse 120% 100% at 20% 10%, #3D0B20 0%, #1A0510 40%, #0E030A 100%);
  position: relative;
  overflow: hidden;
  padding: 1.5rem;
}

/* Ambient orbs */
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
}
.orb-1 {
  width: 55vw; height: 55vw;
  top: -15%; left: -10%;
  background: radial-gradient(circle, rgba(107,26,53,0.55) 0%, transparent 70%);
  animation: drift1 18s ease-in-out infinite;
}
.orb-2 {
  width: 45vw; height: 45vw;
  bottom: -10%; right: -8%;
  background: radial-gradient(circle, rgba(192,71,106,0.35) 0%, transparent 70%);
  animation: drift2 22s ease-in-out infinite;
}
.orb-3 {
  width: 30vw; height: 30vw;
  top: 40%; left: 55%;
  background: radial-gradient(circle, rgba(212,168,83,0.18) 0%, transparent 70%);
  animation: drift3 15s ease-in-out infinite;
}
@keyframes drift1 { 0%,100%{transform:translate(0,0)} 50%{transform:translate(4%,6%)} }
@keyframes drift2 { 0%,100%{transform:translate(0,0)} 50%{transform:translate(-5%,-4%)} }
@keyframes drift3 { 0%,100%{transform:translate(0,0)} 50%{transform:translate(3%,-7%)} }

/* Floating petals */
.petals { position: absolute; inset: 0; pointer-events: none; }
.petal {
  position: absolute;
  top: -40px;
  animation: fall linear infinite;
  filter: drop-shadow(0 0 6px rgba(242,184,198,0.5));
}
@keyframes fall {
  0%   { transform: translateY(-40px) rotate(0deg); opacity: 0; }
  10%  { opacity: 1; }
  90%  { opacity: 0.6; }
  100% { transform: translateY(110dvh) rotate(360deg); opacity: 0; }
}

/* Grain */
.grain {
  position: absolute; inset: 0; pointer-events: none; opacity: 0.035;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 200px 200px;
}

/* Card */
.card {
  width: var(--card-w);
  background: linear-gradient(145deg, rgba(255,248,242,0.13) 0%, rgba(253,232,239,0.09) 100%);
  backdrop-filter: blur(28px) saturate(180%);
  -webkit-backdrop-filter: blur(28px) saturate(180%);
  border: 1px solid rgba(242,184,198,0.22);
  border-radius: 28px;
  position: relative;
  overflow: hidden;
  box-shadow:
    0 0 0 1px rgba(255,255,255,0.07) inset,
    0 40px 80px -20px rgba(0,0,0,0.6),
    0 0 60px rgba(192,71,106,0.12);
}
.card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255,210,220,0.4), transparent);
}

.card-inner {
  padding: 2.4rem 2rem;
}

/* Step transitions */
.step-enter-active { transition: all 0.55s cubic-bezier(0.22, 1, 0.36, 1); }
.step-leave-active { transition: all 0.3s ease-in; }
.step-enter-from { opacity: 0; transform: translateY(22px) scale(0.97); }
.step-leave-to   { opacity: 0; transform: translateY(-14px) scale(0.98); }

/* Shared component tokens */
.title {
  font-family: var(--serif);
  color: #FDE8EF;
  font-weight: 300;
  line-height: 1.15;
  letter-spacing: 0.01em;
}
.subtitle {
  font-family: var(--sans);
  color: rgba(255,210,220,0.65);
  font-weight: 300;
  font-size: 0.85rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.body-text {
  font-family: var(--sans);
  color: rgba(255,220,230,0.78);
  font-size: 0.95rem;
  line-height: 1.65;
  font-weight: 300;
}
.divider {
  width: 48px; height: 1px;
  background: linear-gradient(90deg, transparent, rgba(212,168,83,0.6), transparent);
  margin: 0 auto;
}
.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: linear-gradient(135deg, #C0476A 0%, #8B2145 100%);
  color: #FDE8EF;
  font-family: var(--sans);
  font-size: 0.88rem;
  font-weight: 500;
  letter-spacing: 0.04em;
  padding: 0.85rem 2rem;
  border-radius: 50px;
  border: none;
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 8px 24px rgba(192,71,106,0.35), 0 0 0 1px rgba(255,180,200,0.15) inset;
}
.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 14px 32px rgba(192,71,106,0.45), 0 0 0 1px rgba(255,180,200,0.2) inset;
}
.btn-primary:active { transform: translateY(0); }
</style>