<template>
  <div>
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in answers" 
          :key="index"
          @click="selectedAnswer(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null
    }
  },
  computed: {
    answers() {
      return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
    }
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
      console.log(this.selectedIndex);
      
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
</style>
