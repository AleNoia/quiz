<template>
  <section>
    <p>{{ correctAnswers.length }}</p>
    <p>{{ incorrectAnswers.length }}</p>
  </section>
</template>

<script>
import eventBus from '../eventBus'

export default {
  data() {
    return {
      correctAnswers: [],
      incorrectAnswers: [],
    }
  },
  created(){
    eventBus.$on('dataAnswer', (data) => {
        if(data === true) this.correctAnswers.push(data);
        if(data === false) this.incorrectAnswers.push(data);
    })
  }

}
</script>

<style>

</style>