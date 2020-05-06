<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Fancy quiz</b-nav-item>
      <b-nav-item disabled>Counter {{ correctAnswers }}/{{ currentQuestionNo + 1 }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    currentQuestionNo: Number,
    correctAnswers: Number,
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
