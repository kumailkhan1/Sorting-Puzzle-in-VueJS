<template>
  <div id="app">
    <Header />
    <UserInput v-on:get-name-number="getNameNumber" />

    <!-- <Grid> -->
    <draggable @change="handleChange" class="grid" v-model="numbersArray">
      <v-card
        v-for="num in numbersArray"
        :key="num.index"
        class="gridCell"
        :style="changeWidth"
      >{{num}}</v-card>
    </draggable>
    <div>
      <table>
        <thead>
          <tr>
            <th>Player Name</th>
            <th>Score</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="row in highScorers" :key="row.index">
            <td v-for="col in columns" :key="col.index">{{row[col]}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Header from "./components/layout/Header";
import UserInput from "./components/userInput";
import draggable from "vuedraggable";
import Vue from "vue";
import Vuetify from "vuetify";
import "vuetify/dist/vuetify.min.css";
Vue.use(Vuetify);

export default {
  name: "App",
  components: {
    Header,
    UserInput,
    draggable,
  },

  data() {
    return {
      number: 0,
      timeStarted: null,
      endTime: null,
      numbersArray: [],
      playerName: null,
      highScorers: JSON.parse(localStorage.getItem("TopScorers")),
    };
  },
  computed: {
    changeWidth() {
      return {
        height: 600 / this.number + "px",
        width: 600 / this.number + "px",
      };
    },
    columns: function columns() {
      if (this.highScorers.length == 0) {
        return [];
      }
      return Object.keys(this.highScorers[0]);
    },
  },
  methods: {
    getNameNumber: function (number, name) {
      if (number == "" || number == 0 || name == "") {
        alert("Please enter a valid name or number!");
      } else {
        this.number = number;
        this.playerName = name;
        this.initArray();
      }
    },
    initArray: function () {
      this.timeStarted = new Date();
      let count = 0;
      while (count < this.number * this.number) {
        let randomNum = Math.floor(
          Math.random() * (this.number * this.number) + 1
        );
        if (this.numbersArray.indexOf(randomNum) === -1) {
          this.numbersArray.push(randomNum);
          count++;
        }
      }
    },
    handleChange: function ({ moved }) {
      if (moved) {
        let sorted = true;
        console.log(this.numbersArray);
        for (let i = 0; i < this.numbersArray.length - 1; i++) {
          if (this.numbersArray[i] > this.numbersArray[i + 1]) {
            sorted = false;
            break;
          }
        }
        if (sorted == true) {
          this.endTime = new Date();
          let diff = this.endTime.getTime() - this.timeStarted.getTime();
          diff = diff / 1000;
          if (JSON.parse(localStorage.getItem("TopScorers")) === null) {
            this.highScorers = [];
            this.highScorers.push({ name: this.playerName, score: diff });
            // console.log(highScorers);
            localStorage.setItem(
              "TopScorers",
              JSON.stringify(this.highScorers)
            );
          } else {
            this.highScorers = [];
            let prevObject = JSON.parse(localStorage.getItem("TopScorers"));
            for (let i = 0; i < prevObject.length; i++) {
              this.highScorers.push(prevObject[i]);
            }
            this.highScorers.push({ name: this.playerName, score: diff });
            this.highScorers.sort((a, b) => (a.score < b.score ? -1 : 1));
            localStorage.setItem(
              "TopScorers",
              JSON.stringify(this.highScorers)
            );
          }
          alert("Welcome!");
        }
      }
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.grid {
  height: 600px;
  width: 600px;
  margin-top: 20px;
  text-align: center;
}
.gridCell {
  margin: 0;
  padding: 0;
  border: none;
  display: inline-block;
  font-size: 5em;
  text-align: center;
}
.gridCell:hover {
  background-color: #2a3d66;
  color: white;
}
table {
  font-family: "Open Sans", sans-serif;
  width: 750px;
  border-collapse: collapse;
  border: 3px solid #44475c;
  margin: 10px 10px 0 10px;
}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #44475c;
  color: #fff;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
}
table th:hover {
  background: #717699;
}
table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7d82a8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #d4d8f9;
}
</style>
