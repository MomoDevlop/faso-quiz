<!-- Orthers components must be include here-->

<template>

    <div id="app">
    <navigation
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />
    <div class="row">
      <div class="col-xs-12 m-5" v-show="isEnd">
        <button class="btn btn-succes">
          Vous n'avez droit qu'à 10 questions!! Veuillez cliquer sur <strong> Reset </strong> pour recommencer
        </button>

      </div>
      <div class="col-xs-12 m-5">
        Votre score est de <strong> {{numCorrect}} / 10 </strong>
      </div>
    </div>
    <b-container class="bv-example-row">
        <b-row>
          <b-col sm="6" offset="3">
             <questionBox
              v-if="questions.length"
              :currentQuestion="questions[index]"
              :next="next"
              :increment="increment"
              :isEnd="isEnd"
             />
          </b-col>
        </b-row>
    </b-container>
  </div>

</template>

<script>

import navigation from './components/navigation'
import questionBox from './components/questionBox'

export default {
  name: 'App',
  components: {
    navigation,
    questionBox
  },
  data(){
    return{
      isEnd : false,
      questions: [],
      correctAnswer:'',
      incorrectAnswers:[],
      index:0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next(){
      this.index++
      if(this.index > 9) {
        this.isEnd = true;
      }

    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }

  },

  mounted:  function(){
    // To fetch data from an API
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
      method : 'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData)=>{
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
