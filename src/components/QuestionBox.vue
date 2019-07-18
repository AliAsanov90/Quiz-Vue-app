<template>
  <div>
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers" 
          :key="index"
          @click="selectedAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null
    }
  },
  computed: {
    answers() {
      return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
        this.answered = false
      }
    }
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers) 
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      if (this.selectedIndex) {
        let isCorrect = false
        if (this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.increment(isCorrect)

        this.answered = true
      }
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index)
        answerClass = 'selected'
      else if (this.answered && this.correctIndex === index) 
        answerClass = 'correct'
      else if (this.answered && this.selectedIndex === index && this.correctIndex !== index)
        answerClass = 'incorrect'

      return answerClass
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 20px;
  }

  .list-group-item:hover {
    cursor: pointer;
    background-color: #eee;
  }

  .btn {
    margin: 0 5px;
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: red;
  }
</style>
