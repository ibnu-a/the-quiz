<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "Apa yang menjadi elemen dasar pembentukan air?",
    answer: 0,
    options: [
      "Hidrogen dan Oksigen",
      "Karbon dan Hidrogen",
      "Nitrogen dan Oksigen",
      "Hidrogen dan Karbon",
    ],
    selected: null,
  },
  {
    question:
      "Apakah yang akan terjadi jika Anda berada di dalam lift dan melempar bola pingpong ke atas?",
    answer: 3,
    options: [
      "Bola akan kembali ke tangan Anda",
      "Bola akan mengapung di tengah-tengah lift",
      "Tidak ada yang terjadi",
      "Bola akan jatuh ke lantai",
    ],
    selected: null,
  },
  {
    question: "Apakah nama kimia dari vitamin C?",
    answer: 1,
    options: [
      "Asam Pantotenat",
      "Asam Askorbat",
      "Asam Nikotinat",
      "Asam Folat",
    ],
    selected: null,
  },
  {
    question: "Apa yang bisa berjalan tetapi tidak pernah pergi ke mana pun?",
    answer: 1,
    options: ["Kaki", "Bayangan", "Pikiran", "Waktu"],
    selected: null,
  },
  {
    question:
      'Apa nama senyawa kimia yang dijuluki "pembunuh universal" karena sifatnya yang sangat reaktif dan berpotensi merusak lapisan ozon?',
    answer: 0,
    options: [
      "Klorofluorokarbon (CFC)",
      "Metana",
      "Karbon Dioksida (CO2)",
      "Nitrogen Dioksida (NO2)",
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const setAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <!-- <span class="score">
          {{ getCurrentQuestion.index + 1 }}
        </span> -->
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Selesai"
            : getCurrentQuestion.selected == null
            ? "Pilih Jawaban"
            : "Pertanyaan Selanjutnya"
        }}
      </button>
    </section>
    <section v-else>
      <div v-if="score == questions.length">
        <h1>Wow! Anda benar-benar menguasai semua pertanyaan!</h1>
        <p>
          Skor sempurna! {{ score }} dari {{ questions.length }} Selamat atas
          keberhasilan Anda!
        </p>
      </div>
      <div v-else>
        <h1>Selamat! Anda telah menyelesaikan quiz dengan baik!</h1>
        <p>Skor Anda adalah {{ score }} dari {{ questions.length }}</p>
      </div>
    </section>
  </main>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}
body {
  background-color: #271c36;
  color: #fff;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}
h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  width: 640px;
  border-radius: 0.5rem;
}
.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.quiz-info,
.question {
  color: #8f8f8f;
  font-size: 1.25rem;
}
.quiz-info,
.score {
  color: #fff;
  font-size: 1.25rem;
  margin-left: 1rem;
}
.options {
  margin-bottom: 1rem;
}
.option {
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.option:hover {
  background-color: #2d213f;
}
.option.correct {
  background-color: #2cce7d;
}
.option.wrong {
  background-color: #ff5a5f;
}
.option:last-of-type {
  margin-bottom: 0;
}
.option.disabled {
  opacity: 0.5;
}
.option input {
  display: none;
}
button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 1rem;
  border-radius: 0.5rem;
}
button:disabled {
  opacity: 0.5;
}
h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p {
  color: #8f8f8f;
  font-size: 1.5rem;
}
</style>
