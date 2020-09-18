<template>
  <div>
    <form @submit.prevent="sendNumber">
      <input type="text" v-model="playerName" placeholder="Enter your name..."/>
      <input type="number" v-model="number" name="title" placeholder="Enter a number..." />
      <input type="submit" value="Submit" class="btn" />
    </form>
  </div>
</template>

<script>
export default {
  name: "UserInput",
  data() {
    return {
      number: "",
      playerName:""
    };
  },
  methods:{
      sendNumber(){
          //Sending number to parent
         
          this.$emit("get-name-number",this.number,this.playerName);
      }
  }
};
</script>

<style scoped>
form {
  margin-top: 5px;
  display: flex;
  flex-direction: row;
}
input[type="number"] {
  width: 200px;
  height: 30px;
  padding: 5px;
}
input[type="submit"] {
  width: 200px;
  height: 30px;
  padding: 5px;
  margin-left: 3px;
}

.btn {
  display: inline-block;
  border: none;
  background: #5d54a4;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
  width: 50%;
}
.btn:hover {
  background: #9d65c9;
}
</style>