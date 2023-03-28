<template>
  <div class="question">
    <p>{{ data.question }}</p>
    <QuizAnswer
      v-for="(answer, index) in answers"
      :key="index"
      :data="answer"
      :correctAnswer="data.correct_answer"
      @answer-click="handleAnswerClick"
    />
  </div>
</template>

<script>
import QuizAnswer from './answer.vue';

export default {
  name: 'QuizQuestion',
  components: {
    QuizAnswer,
  },
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  computed: {
    answers() {
      const allAnswers = this.data.incorrect_answers.concat(this.data.correct_answer);
      return this.shuffleArray(allAnswers);
    },
  },
  methods: {
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    handleAnswerClick(isCorrect) {
      this.$emit('answer-selected', isCorrect);
    },
  },
};
</script>


<style scoped>
.question {
  font-size: 18px;
  margin-bottom: 20px;
}

.question p {
  font-weight: bold;
}
</style>

