<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="index"
          @click="selectedAnswer(index)"
          :class="selectedIndex===index ?'selected':''"
        >{{answer}}

        </b-list-group-item>
      </b-list-group>

      <b-button
      variant="primary"
      >
        Submit
      </b-button>

      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>


<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data(){
    return  {
      selectedIndex: null,
      shuffleAnswers: []
    }
  },
  watch:{
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
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

      console.log('mohamed')
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffleAnswers = _.shuffle(answers)

    }
  },
  mounted() {
      this.mixAnswers()
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
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