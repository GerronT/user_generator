<template>
  <div class="hello">
    <!--<img v-bind:src='picture' alt="user_pic">-->
    <input type="text" placeholder="Enter number of users (max: 5000)">
    <input type="submit" value="Generate">
    <h1>Number of Generated Users: {{length}}</h1>
    <p>Sort:</p>
    <select dir="rt1" id="sort" name="sort">
      <option value="" disabled selected>Select</option>
      <option value="alphabetical-username">By Username</option>
    </select>
    <table class="table">
            <thead>
                <tr>
                    <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in items" :key="index">
                    <td>{{index + 1}}</td>
                    <td>{{ item.title }}</td>
                    <td>{{ item.firstName }}</td>
                    <td>{{ item.lastName }}</td>
                    <td>{{ item.userName }}</td>
                    <td><img v-bind:src='item.photo' alt="user_pic"></td>
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
      length: null,
      columns: ['#', 'Title', 'First Name', 'Last Name', 'Username', 'Photo']
    }
  },
  mounted () {
    if (this.length > 0) {
    axios.get('https://randomuser.me/api/?results=' + this.length)
    .then(response => (
      this.items = response.data.results.map(function (item, index) {
        let temp = {
          ID: index + 1,
          title: item.name.title,
          firstName: item.name.first,
          lastName: item.name.last,
          userName: item.login.username,
          photo: item.picture.large
        };
        return temp;
      })
      ))
    }
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

input[type=text]{
  width: 30%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

select {
  width: 15%;
  padding: 5px 5px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-left: 5px;
}

p {
  font-weight: 700;
  font-size: 15px;
  display: inline-block;
}

input[type=submit] {
  width: 10%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}

input[type=submit]:hover {
  background-color: #45a049;
}
</style>
