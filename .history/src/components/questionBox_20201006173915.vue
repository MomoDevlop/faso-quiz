<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>

        <b-list-group-item
          v-for="(answer,index) in shuffleAnswers"
          :key="index"
          @click="selectedAnswer(index)"
          :class="answerClass(index)"
         >
        {{answer}}
        </b-list-group-item>

      </b-list-group>

      <b-button
      variant="primary"
      @click="submitAnswer()"
      :disabled="this.selectedIndex === null || this.isSubmit"
      >
        Submit
      </b-button>

      <b-button
      variant="success"
      :disabled="isEnd"
      href="#"
      @click="next">
       Next
      </b-button>
      <b-button
      variant="danger"
      href="#"
      @click="relaodPage()">
       Reset
      </b-button>
    </b-jumbotron>
  </div>
</template>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>


<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data(){
    return  {
      selectedIndex: null,
      correctIndex: null,
      isSubmit: false,
      shuffleAnswers: [],
      answers : []
    }
  },
  watch:{
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.isSubmit = false
        this.mixAnswers()

      }
    }
  },
  methods: {
    selectedAnswer (index) {
      this.selectedIndex = index
      console.log(this.selectedIndex)
    },

    mixAnswers () {
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffleAnswers = _.shuffle(answers)
      this.correctIndex = _.indexOf(this.shuffleAnswers, this.currentQuestion.correct_answer)

    },
    submitAnswer () {
      let isCorrect = false
      this.isSubmit = true

      if(this.shuffleAnswers[this.selectedIndex] ===this.currentQuestion.correct_answer){
        isCorrect = true
      }
      this.increment(isCorrect)
    },
    relaodPage () {
      window.location.reload()
    },
    /*
    *@params index {number}
    *@retrun String
    */
    answerClass (index) {
      let questionAnswersClass = ''
        if ( !this.isSubmit && this.selectedIndex === index) {
          questionAnswersClass = 'selected'
        } else if (this.isSubmit && this.correctIndex === index ) {
          questionAnswersClass = 'correct'
        } else if (this.isSubmit && this.selectedIndex == index
          && this.correctIndex !== index ) {
          questionAnswersClass = 'incorrect'
        }

        return questionAnswersClass
    }
  },
  mounted() {
      this.mixAnswers()
  },
  computed: {
    answersBox() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      this.answers = answers
      return this.answers;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
  cursor:pointer
}

.btn {
  margin: 05px;
}
.selected{
  background-color: lightblue;
}
.correct{
   background-color: lightgreen
}
.incorrect{
  background-color: lightcoral;
}
</style>