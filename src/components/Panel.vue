<template>
  <section class="panel">
    <b-container fluid="md" class="is-flex">
      <div class="content">
        <div class="is-flex">
          <div class="dataQuestionLength mr-1">
            <div>
              <span>{{ dataQuestionsLength }}</span>
              <i class="fas fa-list-ul"></i>
            </div>
            <p>Questions</p>
          </div>
          <div class="is-flex-column">
            <div class="correctData"><i class="fas fa-check-circle"></i>{{ correctAnswers.length }}</div>
            <div class="correctData"><i class="fas fa-times-circle"></i>{{ incorrectAnswers.length }}</div>
          </div>
        </div>
        <div class="clock">
          <i class="fas fa-stopwatch"></i>
          <span>{{ chronometer }}</span>
        </div>
      </div>
    </b-container>
  </section>
</template>

<script>
  import eventBus from '../eventBus'

  export default {
    data() {
      return {
        correctAnswers: [],
        incorrectAnswers: [],
        dataQuestionsLength: 0,
        remaining: Number,
        time: undefined,
        start: false,
        chronometer: '0:00',
        restart: false,
      }
    },
    methods: {
      // Acrescenta um zero antes do números se for menor que 10
      checkTime(i) {
        return (i < 10) ? "0" + i : i;
      },

      // Inicia a contagem
      startClock() {  
        this.time = (this.dataQuestionsLength * 1000) * 60 //Para cada questão haverá um minuto
        let loop =setInterval(() => {
          this.clock()
          this.time = this.time - 1000
          if(this.time === -1000 || this.restart === true) {
            clearInterval(loop);
            this.restart = false;
          }
        }, 1000)
      },

      // Insere a hora formatada na variável
      clock() {
        let date = new Date(this.time);
        let m = this.checkTime(date.getMinutes());
        let s = this.checkTime(date.getSeconds());
        this.chronometer = `${m}:${s}`
      },
    },
    watch: {
      // Se o chronometer chegar a zero ou nao faltar mais questoes ele irá chamar os resultados
      time() {
        if (this.time === -1000 || this.remaining === 1) {
          eventBus.$emit('showResults', {
            'correctAnswers': this.correctAnswers,
            'incorrectAnswers': this.incorrectAnswers,
            'dataQuestionsLength': this.dataQuestionsLength
          })
          this.time = 10;
          this.remaining = 10;
        }
      }
    },
    created() {
      // Recebe a resposta sendo verdadeira ou falsa 
      eventBus.$on('dataAnswer', (data) => {
        if (data.correct === true) this.correctAnswers.push(data.correct);
        if (data.correct === false) this.incorrectAnswers.push(data.correct);
        this.remaining = data.remaining;
      })

      // Recebe a quantidade de questões
      eventBus.$on('dataQuestions', (data) => {
        this.dataQuestionsLength = data
      })

      // Inicia a contagem se o usuário apertar start
      eventBus.$on('startQuiz', (data) => {
        this.start = data
        this.startClock()
      })

      // Reiniciando app
      eventBus.$on('restartApp', (data) => {
        if (data) {
          this.correctAnswers = [];
          this.incorrectAnswers = [];
          this.startClock()
          this.restart = true;
        }
      })
    }

  }
</script>