<template>
  <div id="app">
      <Panel/>
    <b-container fluid="md">
      <Question />
    </b-container>
      <Footer />
  </div>
</template>

<script>
  import Panel from './components/Panel.vue'
  import Question from './components/Question.vue'
  import Footer from './components/Footer.vue'

  export default {
    name: 'App',
    components: {
      Panel,
      Question,
      Footer
    }
  }
</script>

<style src="./assets/SCSS/mainUseful.css" lang="css" />