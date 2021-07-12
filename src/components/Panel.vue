<template>
  <section class="panel">
    <b-container fluid="md" class="is-flex">
      <div class="is-flex">
        <div class="dataQuestionLength mr-2">
          <div>
            <span>{{ dataQuestionsLength }}</span>
            <i class="fas fa-pencil-alt"></i>
          </div>
          <p>Questions</p>
        </div>
        <div class="is-flex-column">
          <div class="mr-2"><i class="fas fa-check-circle"></i>{{ correctAnswers.length }}</div>
          <div><i class="fas fa-times-circle"></i>{{ incorrectAnswers.length }}</div>
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
        dataQuestionsLength: Number,
      }
    },
    created() {
      eventBus.$on('dataAnswer', (data) => {
          if (data === true) this.correctAnswers.push(data);
          if (data === false) this.incorrectAnswers.push(data);
        }),
        eventBus.$on('dataQuestions', (data) => {
          this.dataQuestionsLength = data
        })
    }

  }
</script>

<style>

</style>