<template>
  <div class="hello">
    <img v-bind:src='picture' alt="user_pic">
    <h1>Title: {{title}}</h1>
    <h1>First Name: {{firstName}}</h1>
    <h1>Last Name: {{lastName}}</h1>
    <h1>Username: {{userName}}</h1>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      title: '',
      firstName: '',
      lastName: '',
      userName: '',
      picture: ''
    }
  },
  mounted () {
    axios.get('https://randomuser.me/api/')
    .then(response => (this.title = response.data.results[0].name.title,
    this.firstName = response.data.results[0].name.first,
    this.lastName = response.data.results[0].name.last,
    this.userName = response.data.results[0].login.username,
    this.picture = response.data.results[0].picture.large))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
