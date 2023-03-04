<template>
  <div>
    <b-container>
      <b-row>
        <b-col cols="6" offset="3">
          <b-jumbotron v-if="total < 10">
            <p>{{currentQuestion.question}}</p>
            <hr class="my-4" />
            <b-list-group>
              <b-list-group-item
                @click.prevent="selectAnswer(index)"
                v-for="(answer, index) in shuffledAnswers"
                :disabled="isSelected"
                :key="index"
                :class="[isSelected && index === selectedIndex && index === correctIndex
                  ? 'correct'
                  : isSelected && index === selectedIndex
                    ? 'incorrect'
                    : isSelected && index === correctIndex
                      ? 'correct'
                      : ''
                  ]"
              >{{answer}}</b-list-group-item>
            </b-list-group>
            <b-button :disabled="selectedIndex === null" variant="success" @click="next">Next</b-button>
          </b-jumbotron>
          <b-jumbotron v-else>
            <p>You have finished the quiz</p>
            <h1>{{correctNum}}/{{total}}</h1>
            <b-button variant="success" @click="reset">Try again</b-button>
          </b-jumbotron>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: [
    "currentQuestion",
    "next",
    "increment",
    "reset",
    "total",
    "correctNum"
  ],
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.isSelected = false;
        this.shuffleAnswers();
      }
    }
  },
  computed: {
    answers() {
      return [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
    }
  },
  methods: {
    selectAnswer(index) {
      let isCorrect = false;
      this.selectedIndex = index;
      if (index === this.correctIndex) {
        isCorrect = true;
      }

      this.isSelected = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      this.shuffledAnswers = _.shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ]);

      this.correctIndex = this.shuffledAnswers.findIndex(
        item => item === this.currentQuestion.correct_answer
      );
      console.log("this.correctIndex :", this.correctIndex);
    }
  },
  data() {
    return {
      isSelected: false,
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: []
    };
  }
};
</script>

<style scoped>
.jumbotron {
  margin-top: 24px;
}

.list-group {
  margin: 15px;
}

.list-group-item:hover {
  background: #aaa;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background: lightblue !important;
}

.correct {
  background: green !important;
  color: white !important;
}

.incorrect {
  background: red !important;
  color: white !important;
}
</style>