<template>
  <div id="app">
      <ModalStart/>
      <ModalResults/>
      <Panel/>
    <b-container fluid="md">
      <Question />
    </b-container>
      <Footer />
  </div>
</template>

<script>
  import Panel from './components/Panel.vue'
  import ModalStart from './components/ModalStart.vue'
  import ModalResults from './components/ModalResults.vue'
  import Question from './components/Question.vue'
  import Footer from './components/Footer.vue'

  export default {
    name: 'App',
    components: {
      ModalStart,
      ModalResults,
      Panel,
      Question,
      Footer,
    }
  }
</script>

<style src="./assets/SCSS/mainUseful.css" lang="css" />