<template>
  <div id="app">
    <Header 
      :numCorrect="numberCorrect"
      :numTotal="numberTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="8" offset="2">
          <QuestionBox 
            v-if="questions.length != 0"
            :currentQuestion="questions[questionIndex]"
            :nextQuestion="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      questionIndex: 0,
      numberCorrect: 0,
      numberTotal: 0,
    }
  },
  methods: {
    next() {
      this.questionIndex ++
    },
    increment(isCorrect){
      if(isCorrect) {
        this.numberCorrect++
      }
      this.numberTotal++
    }
  },
  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&category=28&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json()
    })
    .then((jsonData) => {
      this.questions = jsonData.results
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
