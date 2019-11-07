<template>
  <div class="hello">
    <div class="tab">
      <button class="tablinks" v-on:click="switchTabs(event, 'external-API')">randomuser.me</button>
      <button class="tablinks" v-on:click="switchTabs(event, 'internal-API')">use internal API</button>
    </div>

    <div id="external-API" class="tabcontent">
      <input id="inputNum" type="text" placeholder="Enter number of users (max: 5000)" v-model="inputLength">
      <input type="submit" value="Generate" v-on:click="generate">

      <h1>Number of Generated Users: {{length}}</h1>
    </div>

    <div id="internal-API" class="tabcontent">
      <input type="submit" value="Generate" v-on:click="generateOwn">
    </div>

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
        )).catch(function(error) {
            if (error.response) {
              alert(error.response.data);
              alert(error.response.status);
            } else {
              alert(error);
            }
        })
      } else {
        alert("Please enter a number between 1 to 5000");
      }
    },
    generateOwn: function() {
        axios.get('http://127.0.0.1:8000/users').then(response => (
          this.items = response.data.results.map(function (item, index) {
            let temp = {
              id: index + 1,
              title: item.title,
              firstName: item.firstName,
              lastName: item.lastName,
              userName: item.userName,
              photo: item.photo
            };
            return temp;
          })
        )).catch(function(error) {
            if (error.response) {
              alert(error.response.data);
              alert(error.response.status);
            } else {
              alert(error);
            }
        })
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
    },
    // Switches between internal API and external API (https://www.w3schools.com/howto/howto_js_tabs.asp)
    switchTabs: function(evt, cityName) {
      // Declare all variables
      var i, tabcontent, tablinks;

      // Get all elements with class="tabcontent" and hide them
      tabcontent = document.getElementsByClassName("tabcontent");
      for (i = 0; i < tabcontent.length; i++) {
        tabcontent[i].style.display = "none";
      }

      // Get all elements with class="tablinks" and remove the class "active"
      tablinks = document.getElementsByClassName("tablinks");
      for (i = 0; i < tablinks.length; i++) {
        tablinks[i].className = tablinks[i].className.replace(" active", "");
      }

      // Show the current tab, and add an "active" class to the button that opened the tab
      document.getElementById(cityName).style.display = "block";
      evt.currentTarget.className += " active";
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

  input[class=create] {
    width: 10%;
    background-color: #4CAF50;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 50px;
  }

  input[type=submit]:hover {
    background-color: #45a049;
  }

  /* Style the tab https://www.w3schools.com/howto/howto_js_tabs.asp*/
.tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
}

/* Style the buttons that are used to open the tab content */
.tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
}

/* Change background color of buttons on hover */
.tab button:hover {
  background-color: #ddd;
}

/* Create an active/current tablink class */
.tab button.active {
  background-color: #ccc;
}

/* Style the tab content */
.tabcontent {
  display: none;
  padding: 6px 12px;
  border: 1px solid #ccc;
  border-top: none;
}

.tabcontent {
  animation: fadeEffect 1s; /* Fading effect takes 1 second */
}

/* Go from zero to full opacity */
@keyframes fadeEffect {
  from {opacity: 0;}
  to {opacity: 1;}
}

</style>
