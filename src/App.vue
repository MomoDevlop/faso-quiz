<!-- Orthers components must be include here-->

<template>
  <div id="app">
    <navigation 
      :numCorrect="numCorrect" 
      :index="index"
      :numTotalAnswered="numTotalAnswered" />
    <hr />
    <hr />
    <div class="row">
      <div class="col-xs-12 m-5 alert alert-success">
        Vôtre score est de <strong> {{ numCorrect }} / 10 </strong>
      </div>
    </div>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3" v-if="questions.length">
          <b-alert show variant="warning">
            Il vous reste : {{ remindTime }} sécondes
          </b-alert>
        </b-col>
        <b-col sm="6" offset="3">
          <questionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :timer="timer"
            :isTimerStarted="isTimerStarted"
            :increment="increment"
            :isEnd="isEnd"
            :index="index"
            :showModal="showModal"
            :hideModal="hideModal"
            :fetchData="fetchData"
          />
        </b-col>
        <b-col sm="6" offset="3" v-if="questions.length">
          <b-alert show variant="info">
            <span
              >10 questions au maximun!! Veuillez cliquer sur
              <b-badge href="#" variant="info">Rénitialiser</b-badge> pour
              recommencer
            </span>
          </b-alert>
        </b-col>
        <b-modal ref="my-modal" hide-footer title="Fin des questions">
          <div class="d-block text-center">
            <h3>
              Votre score est de {{ numCorrect }} sur 10, pour un pourcentage de
              {{ Math.round((numCorrect / 10) * 100) }}%100
            </h3>
          </div>
          <b-button
            class="mt-3"
            variant="outline-success"
            block
            @click="hideModal"
            >Fermer</b-button
          >
        </b-modal>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import navigation from "./components/navigation";
import questionBox from "./components/questionBox";

export default {
  name: "App",
  components: {
    navigation,
    questionBox,
  },
  data() {
    return {
      isEnd: false,
      questions: [],
      correctAnswer: "",
      incorrectAnswers: [],
      index: 0,
      numCorrect: 0,
      numTotalAnswered: 0,
      seconds: 20,
      timerId: 0,
      isTimerStarted: false,
    };
  },
  methods: {
    next() {
      if (this.index >= 9) {
        console.log(this.index);
        this.isTimerStarted = false;
        this.resetTimer();
        this.isEnd = true;
        this.showModal();
        return 0
      }

      this.index++;
      this.resetTimer();
      this.timer();
      
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotalAnswered++;
    },

    showModal() {
      this.$refs["my-modal"].show();
    },

    hideModal() {
      this.$refs["my-modal"].hide();
    },
    fetchData() {
      // To fetch data from an API
      fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
        method: "get",
      })
        .then((response) => {
          return response.json();
        })
        .then((jsonData) => {
          this.questions = jsonData.results;
        });
      this.resetDatas()
      this.resetTimer()
    },
    timer() {
      this.idTimer = setInterval(() => {
        this.isTimerStarted = true;
        this.seconds--;
        if (this.seconds === 0) {
          clearInterval(this.idTimer);
          this.seconds = 20;
          this.next();
        }
      }, 1000);
    },
    resetTimer() {
      this.seconds = 20;
      clearInterval(this.idTimer);
    },
    resetDatas() {
      this.isTimerStarted = false
      this.index = 0;
      this.isEnd = false;
      this.numCorrect = 0;
      this.numTotalAnswered = 0;
    },
  },

  mounted: function () {
    this.fetchData();
  },
  computed: {
    remindTime() {
      return this.seconds;
    },
  }
};
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
