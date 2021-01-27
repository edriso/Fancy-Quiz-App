<template>
  <div class="questions-box-container text-center">
    <b-jumbotron>
      <template slot="lead">
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, i) in answers"
          :key="i"
          @click="selectAnswer(i)"
          :class="answerClass(i)"
          ><span v-html="answer"></span
        ></b-list-group-item>
      </b-list-group>

      <b-button @click="nextQuestion" :variant="answered ? 'success' : 'light'">
        Next
      </b-button>

      <b-button
        :variant="answered ? 'light' : 'success'"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionsBox",
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      answered: false,
    };
  },
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function,
  },
  computed: {
    answers() {
      // ... is to copy the array, and then it push the correct answer too
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      answers = _.shuffle(answers);

      return answers;
    },
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.answered = false;
    },

    answers() {
      this.correctIndex = this.answers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
  },
  methods: {
    selectAnswer(i) {
      this.selectedIndex = i;
    },

    answerClass(index) {
      let returnClass = "";

      if (!this.answered && this.selectedIndex === index) {
        returnClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        returnClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        returnClass = "incorrect";
      }

      return returnClass;
    },

    submitAnswer() {
      let isCorrect = false;

      if (
        this.answers[this.selectedIndex] === this.currentQuestion.correct_answer
      ) {
        isCorrect = true;
      }

      this.answered = true;

      this.increment(isCorrect);
    },
  },
  mounted() {
    this.correctIndex = this.answers.indexOf(
      this.currentQuestion.correct_answer
    );
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 1.5rem;
}

.list-group-item:hover {
  cursor: pointer;
  background-color: #eee;
}

.selected {
  background-color: lightblue !important;
}

.correct {
  background-color: lightgreen !important;
}

.incorrect {
  background-color: lightcoral !important;
}

.btn {
  margin: 0 0.5rem;
}

button:focus {
  box-shadow: none;
}
</style>
