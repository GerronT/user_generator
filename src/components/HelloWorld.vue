<template>
  <div class="hello">
    <input id="inputNum" type="text" placeholder="Enter number of users (max: 5000)" v-model="inputLength">
    <input type="submit" value="Generate" v-on:click="generate">

    <h1>Number of Generated Users: {{length}}</h1>

    <p class='label'>Sort:</p>
    <select id="sort" name="sort" @change="sortList($event)" v-model="sort">
      <option value="" disabled selected>--- Select ---</option>
      <option value="alphabetical-username-az">by username (A-Z)</option>
      <option value="alphabetical-username-za">by username (Z-A)</option>
    </select>

    <table class="table">
      <thead>
        <tr>
          <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td>{{ item.id }}</td>
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
      inputLength: null,
      sort: 'default',
      columns: ['#', 'Title', 'First Name', 'Last Name', 'Username', 'Photo']
    }
  },
  methods: {
    // generates a number of users based on user input
    generate: function() {
      if (this.inputLength > 0 && this.inputLength <= 5000) {
        this.length = this.inputLength;
        axios.get('https://randomuser.me/api/?results=' + this.length).then(response => (
          this.items = response.data.results.map(function (item, index) {
            let temp = {
              id: index + 1,
              title: item.name.title,
              firstName: item.name.first,
              lastName: item.name.last,
              userName: item.login.username,
              photo: item.picture.large
            };
            return temp;
          })
        ))
      } else {
        alert("Please enter a number between 1 to 5000");
      }
    },
    // sorts the list of extracted users based on user selection
    sortList: function(event) {
      this.items.sort(function(a, b) {
        a = JSON.stringify(a.userName);
        b = JSON.stringify(b.userName);

        if (event.target.value === 'alphabetical-username-az') {
          return (a < b) ? -1 : (a > b) ? 1 : 0;
        } else if (event.target.value === 'alphabetical-username-za') {
          return (a > b) ? -1 : (a < b) ? 1 : 0;
        }
      })
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
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    margin-left: 5px;
    float: left;
    display: inline-block;
  }

  p[class=label] {
    font-weight: 700;
    font-size: 15px;
    float: left;
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
