<template>
  <div id="app">
    <QuizApp />
  </div>
</template>

<script>
import QuizApp from "./components/quiz.vue";

export default {
  name: "App",
  components: {
    QuizApp,
  },
};
</script>

<style>
/* ここにグローバルなスタイルを定義できます */
</style>
