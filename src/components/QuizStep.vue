<template>
  <div class="quiz">

    <!-- Header — always visible -->
    <div class="quiz-header">
      <p class="subtitle">A little quiz about you, Isha ✨</p>
      <h2 class="title q-main-title">How well does he know you?</h2>
      <div class="divider" style="margin: 0.5rem auto 0;"></div>
    </div>

    <!-- Progress dots -->
    <div class="progress-bar" role="progressbar" :aria-valuenow="qIndex + 1" :aria-valuemax="questions.length">
      <div class="progress-line">
        <div class="progress-fill" :style="{ width: progressPct + '%' }"></div>
      </div>
      <div
        v-for="(q, i) in questions"
        :key="i"
        class="progress-dot"
        :class="{ done: i < qIndex, active: i === qIndex, pending: i > qIndex }"
      >
        <span v-if="i < qIndex" class="dot-check">✓</span>
      </div>
    </div>

    <p class="q-counter subtitle">{{ qIndex + 1 }} / {{ questions.length }}</p>

    <!-- Question card -->
    <Transition name="q-slide" mode="out-in">
      <div class="q-block" :key="qIndex">

        <div class="q-emoji">{{ current.emoji }}</div>
        <h3 class="title q-title">{{ current.question }}</h3>
        <div class="divider" style="margin: 0.5rem auto 0;"></div>

        <!-- Options -->
        <div class="options" role="group">
          <button
            v-for="(opt, i) in current.options"
            :key="i"
            class="opt-btn"
            :class="{
              selected: pickedIndex === i,
              correct:  pickedIndex !== null && opt.correct,
              faded:    pickedIndex !== null && !opt.correct && pickedIndex !== i
            }"
            @click="pick(i)"
            :disabled="pickedIndex !== null"
          >
            <span class="opt-letter">{{ letters[i] }}</span>
            <span class="opt-text">{{ opt.text }}</span>
            <span class="opt-badge" v-if="pickedIndex !== null && opt.correct">✓</span>
            <span class="opt-arrow" v-else>→</span>
          </button>
        </div>

        <!-- Reveal -->
        <Transition name="reveal">
          <div v-if="pickedIndex !== null" class="reveal-card">
            <p class="reveal-msg">"{{ current.options[pickedIndex].reveal }}"</p>
            <button class="btn-primary" @click="advance">
              {{ qIndex < questions.length - 1 ? 'Next →' : 'See your gift 🎁' }}
            </button>
          </div>
        </Transition>

      </div>
    </Transition>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['next'])

const qIndex      = ref(0)
const pickedIndex = ref(null)
const letters     = ['A', 'B', 'C']

const questions = [
  {
    emoji: '🌙',
    question: "He once told you that you are the moon for him. What did you say when he pointed out the dark spots on the moon?",
    options: [
      { text: "You said — so the moon is perfect, just like me 😌", correct: false, reveal: "Ha! A bold answer — but what you actually said was so much more you. Innocent and unknowingly poetic." },
      { text: "You said — I also have some dark spots 🌑", correct: true,  reveal: "And without even realising it, you proved his point completely. He had already told you — you are the moon for him. And then you went ahead and described yourself as one. He teased you instantly, but inside he was completely gone. You are his moon, dark spots and all, and he would not change a single one." },
      { text: "You laughed and changed the topic 😄", correct: false, reveal: "You could never just let a poetic moment pass — your reply was way too good for that." },
    ]
  },
  {
    emoji: '🫓',
    question: "There is one very serious, ongoing argument between you two. What is it about?",
    options: [
      { text: "Whether pineapple belongs on pizza 🍕", correct: false, reveal: "A valid debate — but your actual argument is far more Maharashtrian and far more adorable." },
      { text: "Poli vs Chapati — and you are absolutely right 😤", correct: true,  reveal: "It is Poli. It has always been Poli. He calls it Chapati and every single time, you correct him — as you should. This tiny fight is one of his favourite things about you, because it means you are comfortable enough to argue about bread. That is love." },
      { text: "Who gets the last piece of food 🍽️", correct: false, reveal: "Also a real situation — but the great Poli vs Chapati debate is truly its own chapter." },
    ]
  },
  {
    emoji: '🏠',
    question: "You two have already sorted out your future. If you cook, what does he do?",
    options: [
      { text: "He sets the table nicely 🕯️", correct: false, reveal: "Sweet idea — but the deal is slightly more practical than that." },
      { text: "He cleans the utensils 🫧", correct: true,  reveal: "The deal is made, the roles are assigned. You cook, he cleans. And honestly, the fact that you two have already had these conversations — planning a future together, dividing little duties — that is not nothing. That is everything. He thinks about that future more than you know." },
      { text: "He orders dessert to make up for not cooking 🍰", correct: false, reveal: "He wishes. But no — he has actual responsibilities in this future you planned together." },
    ]
  },
  {
    emoji: '💧',
    question: "At the lake resort on his birthday, something happened that made both your eyes fill with water. What was it?",
    options: [
      { text: "He gave a very emotional speech 🎤", correct: false, reveal: "He is not quite that composed with words in person — what actually happened was quieter and so much more real." },
      { text: "You shared a link — a website you had made, filled with words about him 🌐", correct: true,  reveal: "You sat by the lake, quietly handed him a link, and let your words do what your voice could not. He read it. His eyes filled. Yours did too. And in that moment, you both knew — without saying anything — that what you had was real. That was the moment you truly fell, deeply and completely. And then you said it. Veda ahes kay tu. And you both smiled." },
      { text: "A beautiful song started playing at the right moment 🎵", correct: false, reveal: "The real moment needed no background music. It was perfect exactly as it was." },
    ]
  },
  {
    emoji: '😤',
    question: "He has one habit that you scold him for — every single time — but only with your eyes. What is it?",
    options: [
      { text: "He talks too much on his phone 📱", correct: false, reveal: "Guilty sometimes — but the habit you scold him for is more of a vanity thing." },
      { text: "He keeps fixing his hair, again and again 💇", correct: true,  reveal: "He reaches for his hair, and before his hand even lands — your eyes are already on him. Just a look. No words needed. And somehow, your eyes alone are enough to make him stop completely. He would never admit it, but that look of yours is one of his favourite things in the world." },
      { text: "He hums the same song on repeat 🎶", correct: false, reveal: "Also true — but the scolding-with-eyes thing is reserved for the hair habit specifically." },
    ]
  },
  {
    emoji: '😇',
    question: "He loves catching you in your own words. What does this say about you?",
    options: [
      { text: "You are very sharp and always win arguments 🏆", correct: false, reveal: "You are sharp — but this particular quality is almost the opposite of that, and it is twice as endearing." },
      { text: "You are beautifully innocent and he adores you for it 🤍", correct: true,  reveal: "You say things honestly, openly, without a second thought — and sometimes those very words circle back and he catches you in them. Not to embarrass you. Just because your innocence is one of the things he finds most beautiful about you. You are genuine in a world that rarely is." },
      { text: "You have a terrible memory for what you said 😅", correct: false, reveal: "It is not about memory — it is about how purely and openly you speak. That is the real answer." },
    ]
  },
  {
    emoji: '📞',
    question: "He ends every single call with 'I love you.' You usually do not say it back. Why?",
    options: [
      { text: "You are shy about saying it out loud 🙈", correct: false, reveal: "You are many things — but shy about your feelings is not one of them. The reason is much sweeter." },
      { text: "You are saving it — you said it will always be for his birthday 🎂", correct: true,  reveal: "Every call, every day, he says it. And you stay quiet — not because you do not feel it, but because you are keeping it. Saving the words for the one day they mean the most. His birthday. That is not withholding love. That is the most thoughtful, most Isha thing you could possibly do." },
      { text: "You think actions speak louder than words 💪", correct: false, reveal: "True in general — but you actually have a very specific, very beautiful reason for this one." },
    ]
  },
  {
    emoji: '🌸',
    question: "What does he call you — with that long, unmistakable pronunciation?",
    options: [
      { text: "Ishu 🥺", correct: false, reveal: "Cute — but what he actually calls you is longer, warmer, and completely his own." },
      { text: "Ishaaaaaa — stretched out, like the name itself is not enough 💕", correct: true,  reveal: "Not just Isha. Ishaaaaa. Like saying your name once could never be sufficient. Like he needs to hold onto it a little longer each time. It is not a nickname. It is just the way he says your name when he means everything he never quite says out loud." },
      { text: "Isha ji, very formally 😂", correct: false, reveal: "He respects you deeply — but not quite that formally." },
    ]
  },
  {
    emoji: '💞',
    question: "After everything — the moon, the lake, the poli, the bike, the hair-scolding eyes, the saved I love you — what are you to him?",
    options: [
      { text: "His favourite person 🫶", correct: false, reveal: "True — but even favourite person does not say it fully. The real answer goes further than that." },
      { text: "His life. Simply, completely, his life 🌙", correct: true,  reveal: "Not his girlfriend. Not his favourite person. His life. The one whose existence changed everything — who made him forget his limits, share everything, feel things he did not know how to name. You are not just important to him, Isha. You are the reason the ordinary days feel like something. He loves you. Unconditionally. Always. Happy Birthday. 🎂" },
      { text: "His moon, his home, his calm 🌿", correct: false, reveal: "All of this is true — but the simplest, truest answer is the one that says it all." },
    ]
  },
]

const current     = computed(() => questions[qIndex.value])
const progressPct = computed(() => (qIndex.value / (questions.length - 1)) * 100)

const pick    = (i) => { pickedIndex.value = i }
const advance = () => {
  if (qIndex.value < questions.length - 1) {
    qIndex.value++
    pickedIndex.value = null
  } else {
    emit('next')
  }
}
</script>

<style scoped>
.quiz {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.1rem;
}

/* Header */
.quiz-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
  text-align: center;
}
.q-main-title { font-size: 1.6rem; }

/* Progress bar */
.progress-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  position: relative;
  padding: 0.15rem 0;
  gap: 0;
}
.progress-line {
  position: absolute;
  top: 50%; left: 5%;
  width: 90%;
  height: 1px;
  background: rgba(242,184,198,0.13);
  transform: translateY(-50%);
  z-index: 0;
  border-radius: 2px;
}
.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, rgba(192,71,106,0.8), rgba(212,168,83,0.7));
  border-radius: 2px;
  transition: width 0.5s cubic-bezier(0.4,0,0.2,1);
}
.progress-dot {
  width: 20px; height: 20px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.55rem;
  position: relative;
  z-index: 1;
  transition: all 0.3s ease;
  flex: 1;
  max-width: 28px;
}
.progress-dot.pending {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(242,184,198,0.15);
}
.progress-dot.active {
  background: linear-gradient(135deg, rgba(192,71,106,0.35), rgba(139,33,69,0.25));
  border: 1px solid rgba(192,71,106,0.55);
  box-shadow: 0 0 10px rgba(192,71,106,0.35);
}
.progress-dot.done {
  background: linear-gradient(135deg, rgba(212,168,83,0.28), rgba(192,71,106,0.18));
  border: 1px solid rgba(212,168,83,0.42);
}
.dot-check { color: rgba(212,168,83,0.9); font-size: 0.6rem; font-weight: 600; }
.q-counter { font-size: 0.72rem; letter-spacing: 0.1em; }

/* Question block */
.q-block {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}
.q-emoji {
  font-size: 2.4rem;
  animation: sway 4s ease-in-out infinite;
  filter: drop-shadow(0 0 10px rgba(242,184,198,0.45));
}
@keyframes sway {
  0%,100% { transform: rotate(-4deg) scale(1); }
  50%      { transform: rotate(4deg) scale(1.06); }
}
.q-title { font-size: 1.35rem; text-align: center; line-height: 1.3; }

/* Options */
.options { display: flex; flex-direction: column; gap: 0.55rem; width: 100%; }
.opt-btn {
  width: 100%;
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.8rem 0.95rem;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(242,184,198,0.13);
  border-radius: 13px;
  cursor: pointer;
  transition: all 0.22s ease;
  text-align: left;
}
.opt-btn:not(:disabled):hover {
  background: rgba(192,71,106,0.13);
  border-color: rgba(192,71,106,0.36);
  transform: translateX(4px);
}
.opt-btn.selected {
  background: linear-gradient(135deg, rgba(192,71,106,0.2), rgba(139,33,69,0.14));
  border-color: rgba(192,71,106,0.45);
}
.opt-btn.correct {
  background: linear-gradient(135deg, rgba(212,168,83,0.17), rgba(192,71,106,0.1));
  border-color: rgba(212,168,83,0.48);
  box-shadow: 0 0 12px rgba(212,168,83,0.16);
}
.opt-btn.faded { opacity: 0.28; }
.opt-btn:disabled { cursor: default; }
.opt-letter {
  width: 24px; height: 24px;
  display: flex; align-items: center; justify-content: center;
  border-radius: 50%;
  background: rgba(212,168,83,0.11);
  border: 1px solid rgba(212,168,83,0.26);
  color: rgba(212,168,83,0.85);
  font-family: 'Cormorant Garamond', serif;
  font-size: 0.8rem; font-weight: 600;
  flex-shrink: 0;
}
.opt-text {
  flex: 1;
  color: rgba(255,220,230,0.8);
  font-family: 'DM Sans', sans-serif;
  font-size: 0.86rem; font-weight: 300; line-height: 1.4;
}
.opt-badge { color: rgba(212,168,83,0.9); font-size: 0.85rem; font-weight: 600; flex-shrink: 0; }
.opt-arrow { color: rgba(192,71,106,0.5); font-size: 0.9rem; opacity: 0; transition: opacity 0.2s, transform 0.2s; flex-shrink: 0; }
.opt-btn:not(:disabled):hover .opt-arrow { opacity: 1; transform: translateX(3px); }

/* Reveal */
.reveal-card {
  width: 100%;
  padding: 1.1rem 1rem;
  background: linear-gradient(135deg, rgba(192,71,106,0.12), rgba(107,26,53,0.09));
  border: 1px solid rgba(192,71,106,0.23);
  border-radius: 16px;
  display: flex; flex-direction: column; align-items: center; gap: 0.9rem;
}
.reveal-msg {
  font-family: 'Cormorant Garamond', serif;
  font-style: italic;
  font-size: 1.02rem; font-weight: 300;
  color: rgba(255,220,230,0.88);
  line-height: 1.55; text-align: center;
}

/* Transitions */
.q-slide-enter-active { transition: all 0.42s cubic-bezier(0.22, 1, 0.36, 1); }
.q-slide-leave-active { transition: all 0.22s ease-in; }
.q-slide-enter-from  { opacity: 0; transform: translateX(26px) scale(0.98); }
.q-slide-leave-to    { opacity: 0; transform: translateX(-18px) scale(0.98); }
.reveal-enter-active { transition: all 0.46s cubic-bezier(0.34, 1.56, 0.64, 1); }
.reveal-leave-active { transition: all 0.18s ease-in; }
.reveal-enter-from   { opacity: 0; transform: scale(0.9) translateY(10px); }
.reveal-leave-to     { opacity: 0; }
</style>