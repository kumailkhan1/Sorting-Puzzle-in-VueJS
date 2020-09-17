<template>
  <div id="app">
    <Header />
    <UserInput v-on:get-number="getNumber" />
    <!-- <Grid> -->
    <draggable
      
      @change="handleChange"
      class="grid"
      v-model="numbersArray"
    >
      <v-card
        v-for="num in numbersArray"
        :key="num.index"
        class="gridCell"
        :style="changeWidth"
      >{{num}}</v-card>
    </draggable>
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
      numbersArray: [],
    };
  },
  computed: {
    changeWidth() {
      return {
        height: 600 / this.number + "px",
        width: 600 / this.number + "px",
      };
    },
  },
  methods: {
    getNumber: function (number) {
      this.number = number;
      console.log(this.number);
      this.initArray();
    },
    initArray: function () {
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
    handleChange: function ({moved}) {
      if(moved){
      let sorted = true;
      console.log(this.numbersArray);
      for (let i = 0; i < this.numbersArray.length - 1; i++) {
        if (this.numbersArray[i] > this.numbersArray[i + 1]) {
          sorted = false;
          break;
        }
      }
      if (sorted == true) {
        console.log(this.numbersArray);
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
</style>
