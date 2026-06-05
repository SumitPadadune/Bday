<template>
  <div class="welcome">
    <!-- Envelope icon -->
    <div class="icon-wrap">
      <div class="icon-glow"></div>
      <div class="icon-ring">💌</div>
    </div>

    <!-- Heading -->
    <div class="heading-block">
      <p class="subtitle">A birthday surprise</p>
      <h1 class="title" style="font-size: 2.6rem; text-align:center;">
        Hey Beautiful
      </h1>
      <div class="divider"></div>
    </div>

    <!-- Body -->
    <p class="body-text" style="text-align:center; max-width:300px;">
      Today is a very special day, and I've built a tiny corner of the internet just for you.
      Are you ready to see it?
    </p>

    <!-- Buttons -->
    <div class="btn-row" ref="arena">
      <button class="btn-primary" @click="$emit('next')">
        Yes, I'm ready ✨
      </button>

      <button
        ref="noBtn"
        class="btn-ghost"
        :style="noBtnStyle"
        @mouseenter="flee"
        @touchstart.prevent="flee"
      >
        No 🥺
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue'

defineEmits(['next'])

const noBtn = ref(null)
const arena = ref(null)
const noBtnStyle = reactive({})

const flee = () => {
  const vw = window.innerWidth
  const vh = window.innerHeight
  const bw = noBtn.value?.offsetWidth || 90
  const bh = noBtn.value?.offsetHeight || 44

  const x = Math.max(16, Math.random() * (vw - bw - 32))
  const y = Math.max(16, Math.random() * (vh - bh - 32))

  Object.assign(noBtnStyle, {
    position: 'fixed',
    top: y + 'px',
    left: x + 'px',
    zIndex: 9999,
    transition: 'top 0.18s ease, left 0.18s ease',
  })
}
</script>

<style scoped>
.welcome {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.6rem;
}

/* Icon */
.icon-wrap {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}
.icon-glow {
  position: absolute;
  width: 90px; height: 90px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(192,71,106,0.5) 0%, transparent 70%);
  animation: pulse-glow 3s ease-in-out infinite;
}
@keyframes pulse-glow {
  0%,100% { transform: scale(1); opacity: 0.6; }
  50%      { transform: scale(1.25); opacity: 1; }
}
.icon-ring {
  font-size: 3rem;
  animation: bounce 3.5s ease-in-out infinite;
  filter: drop-shadow(0 0 12px rgba(242,184,198,0.6));
}
@keyframes bounce {
  0%,100% { transform: translateY(0); }
  50%      { transform: translateY(-10px); }
}

/* Heading */
.heading-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
}

/* Buttons */
.btn-row {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  width: 100%;
  padding-top: 0.4rem;
}
.btn-ghost {
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,180,200,0.18);
  color: rgba(255,210,220,0.5);
  font-family: 'DM Sans', sans-serif;
  font-size: 0.85rem;
  padding: 0.75rem 1.8rem;
  border-radius: 50px;
  cursor: pointer;
  transition: color 0.2s;
  user-select: none;
}
.btn-ghost:hover {
  color: rgba(255,210,220,0.7);
}
</style>