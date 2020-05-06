<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <h4>{{ question.question }}</h4>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item @click="selectAnswer(index)" v-for="(item, index) in answers" 
        :key="index"
        :class="answerClass(index)"
        >{{ item }}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered && selectedIndex !== null">Submit</b-button>
      <b-button variant="success" :disabled="!answered || currentQuestionNo===9" @click="nextQuestion">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: "QuestionBox",
  props: {
    question: Object,
    nextQuestion: Function,
    incrementCorrects: Function,
    currentQuestionNo: Number
  },
  data() {
    return {
      answered: false,
      selectedIndex: null,
      correctAnswerIndex: null,
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers, this.question.correct_answer];
      answers = _.shuffle(answers);
      return answers;
    }
  },
  watch: {
    question: {
      immediate: true,
      handler(){
        if(this.question !== undefined) {
          this.answered = false;
          this.selectedIndex = null;
          this.correctAnswerIndex = this.answers.indexOf(this.question.correct_answer);
        }
      }
    }
  },
  methods: {
    selectAnswer(index){
      if(!this.answered) this.selectedIndex = index; 
    },
    answerClass(index) {
      let answerClass = '';
      if(this.selectedIndex === index && !this.answered) {
        answerClass = 'selected'
      } else if (this.answered && index === this.correctAnswerIndex) {
        answerClass =  'correct';
      } else if(this.answered && index !== this.correctAnswerIndex && this.selectedIndex === index) {
        answerClass =  'incorrect';
      }

      return answerClass;
    },
    submitAnswer() {
      this.answered = true;

      if(this.correctAnswerIndex === this.selectedIndex) {
        this.incrementCorrects();
      }
    }
  }
};
</script>

<style scoped>
.question-box-container {
  margin: 50px 0;
}

.list-group-item:hover {
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightsalmon;
}
</style>