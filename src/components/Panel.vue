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
            <div><i class="fas fa-check-circle"></i>{{ correctAnswers.length }}</div>
            <div><i class="fas fa-times-circle"></i>{{ incorrectAnswers.length }}</div>
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
        remaining: undefined,
        time: undefined,
        start: false,
        chronometer: '0:00',
      }
    },
    methods: {
      // Acrescenta um zero antes do números se for menor que 10
      checkTime(i) {
        return (i < 10) ? "0" + i : i;
      },

      // Insere a hora formatada na variável
      clock() {
        let date = new Date(this.time);
        let m = this.checkTime(date.getMinutes());
        let s = this.checkTime(date.getSeconds());
        this.chronometer = `${m}:${s}`
      },

      // Inicia a contagem
      startClock() {
        this.time = (this.dataQuestionsLength * 1000) * 60
        setInterval(() => {
          this.clock()
          this.time = this.time - 1000
        }, 1000)
      }
    },
    watch: {
      // Se o chronometer chegar a zero ele irá chamar os resultados
      time() {
        if (this.time === -1000 || this.remaining === 1) {
          eventBus.$emit('showResults', {
            'correctAnswers': this.correctAnswers,
            'incorrectAnswers': this.incorrectAnswers,
            'dataQuestionsLength': this.dataQuestionsLength
          })
        }
        this.time = 10;
        this.remaining = 0;
      }
    },
    created() {
      // Recebe a respota sendo verdadeira ou falsa 
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
        }
      })
    }

  }
</script>