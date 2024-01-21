<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    questions: "what is Vue JS?",
    answer: 0,
    options: [
      "A framework for building user interfaces",
      "A Library",
      "An ice cream maker",
    ],
    selected: null,
  },
  {
    questions: "what is vuex?",
    answer: 2,
    options: ["Vue with an x", "A bla bla", "state managment library"],
    selected: null,
  },
  {
    questions: "what is vue router used for?",
    answer: 1,
    options: ["walking in space", "A routing library for vue JS", "ta2ta2"],
  },
]);

const quizcompleted = ref(false);
const currentQuestions = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.forEach((q) => {
    if (q.selected === q.answer) {
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestions.value];
  question.index = currentQuestions.value;
  return question;
});

const SetAnswer = (evt) => {
  questions.value[currentQuestions.value].selected = evt.target.value;
  evt.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestions.value < questions.value.length - 1) {
    currentQuestions.value++;
  } else {
    quizcompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>the Quiz</h1>
    <section class="quiz" v-if="!quizcompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">score{{ score }}/{{ questions.length }}</span>
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="{
            option: true,
            correct:
              getCurrentQuestion.selected === index &&
              index === getCurrentQuestion.answer,
            wrong:
              getCurrentQuestion.selected === index &&
              index !== getCurrentQuestion.answer,
            disabled:
              getCurrentQuestion.selected !== null &&
              index !== getCurrentQuestion.selected,
          }"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? " Select an option"
            : "Next Question"
        }}
      </button>
    </section>
    <section v-else>
      <h1>you have finish the quiz</h1>
      <p>your score is {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
.app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  background-color: pink;
  color: black;
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
  background-color: blue;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}
.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.quiz-info .question {
  color: yellow;
  font-size: 1.25rem;
}
.quiz-info .scrore {
  color: blueviolet;
  font-size: smaller;
}
.option {
  margin-bottom: 1rem;
}
.option {
  display: block;
  padding: 1rem;
  background-color: chartreuse;
  margin-bottom: 0.5rem;
  cursor: pointer;
}
.option.correct {
  background-color: green;
}
.option.wrong {
  background-color: red;
}
.option.disabled {
  opacity: 0.5rem;
}
.option input {
  display: none;
}
button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: cadetblue;
  color: cornflowerblue;
  font-weight: 700;
  text-transform: uppercase;
  font-size: small;
  border-radius: 0.5rem;
}
button.disables {
  opacity: 00.5rem;
}
h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p {
  color: darkblue;
  font-size: 1.5rem;
  text-align: center;
}
</style>
