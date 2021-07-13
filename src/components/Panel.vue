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
            <div class="mr-2"><i class="fas fa-check-circle"></i>{{ correctAnswers.length }}</div>
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
        time: undefined,
        chronometer: '0:00',
      }
    },
    methods: {

      checkTime(i) {
        return (i < 10) ? "0" + i : i;
      },

      clock() {
        let date = new Date(this.time);
        let m = this.checkTime(date.getMinutes());
        let s = this.checkTime(date.getSeconds());
        this.chronometer = `${m}:${s}`
      }
    },
    created() {

      eventBus.$on('dataAnswer', (data) => {
        if (data === true) this.correctAnswers.push(data);
        if (data === false) this.incorrectAnswers.push(data);
      })

      eventBus.$on('dataQuestions', (data) => {
        this.dataQuestionsLength = data

        // timer
        this.time = (data * 1000) * 60
        setInterval(() => {
          this.clock()
          this.time = this.time - 1000
        }, 1000)
      })
    }

  }
</script>

<style>

</style>