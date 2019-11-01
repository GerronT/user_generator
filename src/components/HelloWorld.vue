<template>
  <div class="hello">
    <!--<img v-bind:src='picture' alt="user_pic">-->
    <h1>Number of Generated Users: {{length}}</h1>
    <table class="table">
      <thead>
          <tr>
              <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
          </tr>
      </thead>
      <tbody>
          <tr v-for="(item, index) in items" :key="index">
            <td v-for="(column, indexColumn) in columns" :key="indexColumn">{{item[column]}}</td>
          </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      items: [],
      length: '7',
      columns: ['ID', 'Title', 'First_Name', 'Last_Name', 'User_Name', 'Photo']
    }
  },
  mounted () {
    axios.get('https://randomuser.me/api/?results=' + this.length)
    .then(response => (
      this.items = response.data.results.map(function (item, index) {
        let temp = {
          ID: index + 1,
          Title: item.name.title,
          First_Name: item.name.first,
          Last_Name: item.name.last,
          User_Name: item.login.username,
          Photo: item.picture.thumbnail
        };
        return temp;
      })
      ))
  },
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

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
