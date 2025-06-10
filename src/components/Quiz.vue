<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  { question: "Apa kepanjangan dari CPU?", correctAnswer: "Central Processing Unit" },
  { question: "Apa nama sistem operasi yang dikembangkan oleh Apple?", correctAnswer: "macOS" },
  { question: "Apa bahasa pemrograman yang umum digunakan untuk pengembangan web sisi klien?", correctAnswer: "JavaScript" },
  { question: "Perangkat keras apa yang berfungsi sebagai 'otak' komputer?", correctAnswer: "Processor" },
  { question: "Apa nama jaringan global yang menghubungkan miliaran perangkat komputer?", correctAnswer: "Internet" },
  { question: "Apa kepanjangan dari RAM?", correctAnswer: "Random Access Memory" },
  { question: "Siapa pendiri Microsoft?", correctAnswer: "Bill Gates" },
  { question: "Apa nama program jahat yang menyebar dari satu komputer ke komputer lain?", correctAnswer: "Virus" },
  { question: "Apa perangkat penyimpanan data non-volatile yang menggunakan memori flash?", correctAnswer: "SSD" },
  { question: "Apa singkatan untuk 'World Wide Web'?", correctAnswer: "WWW" }
])
const answer = ref("")
const score = ref(0)
const currentIndex = ref(0)
const finished = ref(false)

const currentQuestion = computed(() => {
  if (currentIndex.value < questions.value.length) {
    return questions.value[currentIndex.value]
  }
  return null 
})

const hasMoreQuestions = computed(() => {
  return currentIndex.value < questions.value.length
})

const submitAnswer = (() => {
  if (!hasMoreQuestions.value) {
    return;
  }

  const correctAnswer = currentQuestion.value.correctAnswer

  if (answer.value.toLowerCase() === correctAnswer.toLowerCase()) {
    score.value += 10
  } else {
    score.value -= 5
    if (score.value < 0) score.value = 0 
  }

  currentIndex.value++
  answer.value = "" 

  if (!hasMoreQuestions.value) {
    finished.value = true
  }
}) 
</script>

<template>
  <div>
    <h1>Quiztech</h1>

    <div v-if="!finished">
      <p class="question-text">{{ currentQuestion.question }}</p>
      <form @submit.prevent="submitAnswer">
        <input v-model="answer" type="text" placeholder="Masukkan jawaban Anda..." />
        <button type="submit">Submit</button>
      </form>
      <p class="current-score-display">Skor Saat Ini: {{ score }}</p>
    </div>

    <div v-else class="quiz-results">
      <p class="final-score-text">Skor Akhir Anda: <span class="final-score-value">{{ score }}</span></p>
      <p v-if="score >= 50" class="congrats-message">Selamat! Anda berhasil!</p>
      <p v-else class="try-again-message">Coba lagi lain kali! Tingkatkan pengetahuan Anda.</p>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
h1 {
  font-family: var(--font-family-pixel);
  font-size: calc(var(--pixel-size) * 12);
  color: var(--color-text);
  text-shadow:
    var(--pixel-size) var(--pixel-size) 0 #000;
  margin-bottom: calc(var(--pixel-size) * 8);
  letter-spacing: var(--pixel-size);
}

p {
  font-size: calc(var(--pixel-size) * 7);
  margin-bottom: calc(var(--pixel-size) * 5);
  line-height: 1.5;
}

form {
  display: flex;
  flex-direction: column;
  gap: calc(var(--pixel-size) * 5);
  margin-top: calc(var(--pixel-size) * 10);
}

input[type="text"] {
  background-color: var(--color-input-bg);
  border: var(--pixel-size) solid var(--color-border);
  padding: calc(var(--pixel-size) * 4) calc(var(--pixel-size) * 6);
  font-family: var(--font-family-pixel);
  font-size: calc(var(--pixel-size) * 6);
  color: var(--color-input-text);
  box-shadow:
    var(--pixel-size) var(--pixel-size) 0 #000;
  outline: none;
  transition: all 0.1s ease-in-out;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input[type="text"]:focus {
  border-color: var(--color-primary);
  box-shadow:
    var(--pixel-size) var(--pixel-size) 0 var(--color-primary);
}

button[type="submit"] {
  background-color: var(--color-button-bg);
  color: var(--color-button-text);
  border: var(--pixel-size) solid var(--color-primary);
  padding: calc(var(--pixel-size) * 4) calc(var(--pixel-size) * 8);
  font-family: var(--font-family-pixel);
  font-size: calc(var(--pixel-size) * 7);
  cursor: pointer;
  box-shadow:
    var(--pixel-size) var(--pixel-size) 0 #000;
  transition: all 0.1s ease-in-out;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

button[type="submit"]:hover {
  background-color: rgb(153, 255, 0);
  box-shadow:
    calc(var(--pixel-size) * 2) calc(var(--pixel-size) * 2) 0 #000;
  transform: translate(-var(--pixel-size), -var(--pixel-size));
}

button[type="submit"]:active {
  background-color: #99ec3f;
  box-shadow:
    var(--pixel-size) var(--pixel-size) 0 #000 inset;
  transform: translate(0, 0);
}

.quiz-results {
  margin-top: calc(var(--pixel-size) * 10);
}

.quiz-results h2 {
  font-size: calc(var(--pixel-size) * 10);
  color: var(--color-primary);
  text-shadow: var(--pixel-size) var(--pixel-size) 0 #000;
  margin-bottom: calc(var(--pixel-size) * 5);
}

.quiz-results p {
  font-size: calc(var(--pixel-size) * 8);
  margin-bottom: calc(var(--pixel-size) * 5);
}

.final-score {
  font-size: calc(var(--pixel-size) * 10);
  color: var(--color-secondary);
  font-weight: bold;
}

.congrats-message {
  color: red;
  font-size: calc(var(--pixel-size) * 9);
  animation: pulse 1s infinite alternate;
}

.try-again-message {
  color: red;
  font-size: calc(var(--pixel-size) * 9);
}

@keyframes pulse {
  from {
    transform: scale(1);
    opacity: 1;
  }
  to {
    transform: scale(1.05);
    opacity: 0.9;
  }
}

.pixel-border {
  background-image:
    linear-gradient(to right, var(--color-border) var(--pixel-size), transparent var(--pixel-size)),
    linear-gradient(to right, var(--color-border) var(--pixel-size), transparent var(--pixel-size)),
    linear-gradient(to bottom, var(--color-border) var(--pixel-size), transparent var(--pixel-size)),
    linear-gradient(to bottom, var(--color-border) var(--pixel-size), transparent var(--pixel-size));
  background-size:
    100% var(--pixel-size), 100% var(--pixel-size),
    var(--pixel-size) 100%, var(--pixel-size) 100%;
  background-position:
    0 0, 0 100%,
    0 0, 100% 0;
  background-repeat: no-repeat;
}
</style>