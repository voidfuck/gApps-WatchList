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
          