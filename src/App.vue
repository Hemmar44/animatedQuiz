<template>
  <div id="app">
    <quiz-base></quiz-base>
  </div>
</template>

<script>
import QuizBase from './components/QuizBase'

export default {
  name: 'app',
  components: {
    QuizBase
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
