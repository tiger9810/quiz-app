<template>
  <div class="quiz">
    <div v-if="quizState === 'not-started'">
      <button @click="startQuiz">クイズを開始</button>
    </div>
    <div v-else-if="quizState === 'in-progress'">
      <Question v-if="questions.length" :data="questions[currentQuestionIndex]" @answer-selected="handleAnswerSelected"/>
    </div>
    <div v-else-if="quizState === 'finished'">
      <p>あなたのスコア: {{ score }}/{{ questions.length }}</p>
      <button @click="resetQuiz">もう一度挑戦する</button>
    </div>
  </div>
</template>

<script>
import Question from './question.vue';

export default {
  name: 'QuizApp',
  components: {
    Question,
  },
  data() {
    return {
      quizState: 'not-started',
      questions: [],
      currentQuestionIndex: 0,
      score: 0,
    };
  },
  methods: {
    async fetchQuiz() {
      const response = await fetch('https://opentdb.com/api.php?amount=10&type=multiple');
      const data = await response.json();
      this.questions = data.results;
    },
    startQuiz() {
      this.fetchQuiz();
      this.quizState = 'in-progress';
    },
    handleAnswerSelected(isCorrect) {
      if (isCorrect) {
        this.score++;
      }
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
      } else {
        this.quizState = 'finished';
      }
    },
    resetQuiz() {
      this.quizState = 'not-started';
      this.currentQuestionIndex = 0;
      this.score = 0;
    },
  },
};
</script>

<style scoped>
.quiz {
  font-family: "Arial", sans-serif;
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.quizState {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}
</style>
