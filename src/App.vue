<template>
  <div id="app">
    <UserGenerator msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import UserGenerator from './components/UserGenerator.vue'

export default {
  name: 'app',
  components: {
    UserGenerator
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
