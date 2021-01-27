<template>
  <div id="app">
    <Header :correctAnswers="numCorrect" :totalAnswers="numTotal" />

    <b-container class="mt-2">
      <b-row>
        <b-col sm="6" offset-sm="3" v-if="!finished">
          <questions-box
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="next"
            :increment="incrementCounter"
          />
        </b-col>

        <b-col sm="6" offset-sm="3" v-else>
          <b-jumbotron>
            <template slot="lead">
              <span>Your score is: {{ numCorrect }}</span>
            </template>

            <a href="/">Quiz again?</a>
          </b-jumbotron>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionsBox from "./components/QuestionsBox.vue";

export default {
  name: "App",
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      finished: false,
    };
  },
  components: {
    Header,
    QuestionsBox,
  },
  created() {
    fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
        this.numTotal++;
      } else {
        this.finished = true;
      }
    },

    incrementCounter(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
    },
  },
};
</script>
