<template>
  <div class="quiz">
    <div v-if="quizState === 'not-started'">
      <button @click="startQuiz">クイズを開始</button>
    </div>
    <div v-else-if="quizState === 'in-progress'">
      <Question v-if="questions.length" :data="questions[currentQuestionIndex]" @answer-selected="handleAnswerSelected"/>
      <div v-if="answerResult !== null">
        <p v-if="answerResult">正解！</p>
        <p v-else>不正解！正しい答えは「{{ correctAnswer }}」でした。</p>
        <button @click="goToNextQuestion">次の問題へ</button>
      </div>
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
      answerResult: null,
      correctAnswer: '',
    };
  },
  methods: {
    async fetchQuiz() {
      const response = await fetch('https://opentdb.com/api.php?amount=10&category=31&type=multiple&encode=url3986');
      const data = await response.json();
      this.questions = data.results.map(result => {
        const decodedQuestion = decodeURIComponent(result.question);
        const decodedCorrectAnswer = decodeURIComponent(result.correct_answer);
        const decodedIncorrectAnswers = result.incorrect_answers.map(answer => decodeURIComponent(answer));
        return {
          ...result,
          question: decodedQuestion,
          correct_answer: decodedCorrectAnswer,
          incorrect_answers: decodedIncorrectAnswers,
        }
      });
    },
    startQuiz() {
      this.fetchQuiz();
      this.quizState = 'in-progress';
    },
    handleAnswerSelected(isCorrect, correctAnswer) {
      this.correctAnswer = correctAnswer;
      if (isCorrect) {
        this.score++;
        this.answerResult = true;
      } else {
        this.answerResult = false;
      }
    },
    goToNextQuestion() {
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
        this.correctAnswer = '';
        this.answerResult = null;
      } else {
        this.quizState = 'finished';
      }
    },
    resetQuiz() {
      this.quizState = 'not-started';
      this.currentQuestionIndex = 0;
      this.score = 0;
      this.answerResult = null;
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
