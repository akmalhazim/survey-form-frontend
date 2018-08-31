<template>
  <div id="app">
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <h1>Pre-Course Questionaire</h1>
    <p style="margin:0;font-size:16px;">Application of Windows Servers</p>
    <br>
    <br>
    <br>
    <Form backend="http://survey-backend.test"></Form>
  </div>
</template>

<script>
import Form from './components/form'

export default {
  name: 'app',
  components: {
    Form
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
