<template>
  <div class="video-step">
    <!-- Crown -->
    <div class="crown">👑</div>

    <!-- Heading -->
    <div class="heading-block">
      <p class="subtitle">A message just for you</p>
      <h1 class="title" style="font-size: 2.4rem; text-align:center;">
        Happy Birthday!
      </h1>
      <div class="divider"></div>
    </div>

    <p class="body-text" style="text-align:center; max-width:300px;">
      I made a little video to capture a fraction of how much you mean to me.
    </p>

    <!-- Video player -->
    <div class="video-frame">
      <div class="video-border-glow"></div>
      <video
        src="https://res.cloudinary.com/YOUR_CLOUD_NAME/video/upload/v1234567890/YOUR_VIDEO_PUBLIC_ID.mp4"
        controls
        playsinline
        controlsList="nodownload"
        class="video-el"
      ></video>
    </div>

    <!-- Love tag -->
    <div class="love-tag">
      <span class="love-text">I love you to the moon and back 🌙</span>
    </div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'

onMounted(async () => {
  try {
    const { default: confetti } = await import('https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.2/dist/confetti.module.mjs')
    const end = Date.now() + 4000
    ;(function frame() {
      confetti({ particleCount: 2, angle: 60, spread: 65, origin: { x: 0, y: 0.85 }, colors: ['#FF9B9B','#FFC5C5','#D63484','#FFF0F5','#D4A853'] })
      confetti({ particleCount: 2, angle: 120, spread: 65, origin: { x: 1, y: 0.85 }, colors: ['#FF9B9B','#FFC5C5','#D63484','#FFF0F5','#D4A853'] })
      if (Date.now() < end) requestAnimationFrame(frame)
    })()
  } catch(e) { /* confetti is a bonus */ }
})
</script>

<style scoped>
.video-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.4rem;
}

/* Crown */
.crown {
  font-size: 3rem;
  animation: float-crown 4s ease-in-out infinite;
  filter: drop-shadow(0 0 14px rgba(212,168,83,0.6));
}
@keyframes float-crown {
  0%,100% { transform: translateY(0) rotate(0deg); }
  50%      { transform: translateY(-10px) rotate(4deg); }
}

/* Heading */
.heading-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

/* Video frame */
.video-frame {
  width: 100%;
  position: relative;
  border-radius: 18px;
  overflow: hidden;
}
.video-border-glow {
  position: absolute;
  inset: -2px;
  border-radius: 20px;
  background: linear-gradient(135deg, #C0476A, #D4A853, #8B2145);
  z-index: 0;
}
.video-el {
  position: relative;
  z-index: 1;
  width: 100%;
  aspect-ratio: 16/9;
  display: block;
  border-radius: 16px;
  background: #0a0306;
  object-fit: cover;
  margin: 3px;
  width: calc(100% - 6px);
}

/* Love tag */
.love-tag {
  padding: 0.7rem 1.5rem;
  background: rgba(192,71,106,0.12);
  border: 1px solid rgba(192,71,106,0.25);
  border-radius: 50px;
}
.love-text {
  font-family: 'Cormorant Garamond', serif;
  font-style: italic;
  font-size: 1.05rem;
  font-weight: 300;
  color: rgba(255,210,220,0.85);
  letter-spacing: 0.03em;
}
</style>