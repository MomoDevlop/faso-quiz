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
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>

      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data(){
    return  {
      selectedIndex:null
    }
  },
  watch:{
    currentQuestion(){
      this.selectedIndex=null
      this.shuffleAnswers()
    }
  },
  methods: {
    selectedAnswer(index){
      this.selectedIndex=index
      console.log(this.selectedIndex)
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffleAnswers = _.shuffle(answers)
    }

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