
<template>
  <div id="app">
    <Header :total="total" :correctNum="correctNum" />
    <QuizBox
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
      :reset="reset"
      :total="total"
      :correctNum="correctNum"
    />
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/layout/Header";
import QuizBox from "./components/QuizBox";

export default {
  name: "app",
  components: {
    Header,
    QuizBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      total: 0,
      correctNum: 0
    };
  },
  methods: {
    next() {
      if (this.index < 9) this.index++;
    },

    increment(isCorrect) {
      if (isCorrect) {
        this.correctNum++;
      }

      this.total++;
    },

    reset() {
      this.total = 0;
      this.correctNum = 0;
      this.index = 0;
    }
  },
  mounted() {
    axios
      .get("https://opentdb.com/api.php?amount=10&category=27&type=multiple")
      .then(res => (this.questions = res.data.results))
      .catch(err => console.log(err));
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>