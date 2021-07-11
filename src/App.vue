<template>
  <div id="app">
    <b-container fluid="md">
      <Panel />
      <Question />
      <Footer />
    </b-container>
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