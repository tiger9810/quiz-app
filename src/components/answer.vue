<template>
  <div class="answer" @click="handleClick">
    {{ data }}
  </div>
</template>

<script>
export default {
  name: 'QuizAnswer',
  props: {
    data: {
      type: String,
      required: true,
    },
    correctAnswer: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleClick() {
      const isCorrect = this.data === this.correctAnswer;
      this.$emit('handleClick', isCorrect);
    },
  },
};
</script>

<style scoped>
.answer {
  display: inline-block;
  background-color: #f4f4f4;
  border: 1px solid #ccc;
  padding: 10px 20px;
  margin: 5px;
  cursor: pointer;
  border-radius: 5px;
  transition: all 0.3s ease;
}

.answer:hover {
  background-color: #e0e0e0;
}
</style>
