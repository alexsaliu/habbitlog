<template>
  <div id="app">
    <div class="habbitlog">HabbitLog</div>
    <div v-if="storedName">
      <div class="name">{{storedName}}</div>
      <div class="change-name" @click="storedName = false">New Account</div>
      <div :style="{color: removeHabbits ? 'red' : ''}" class="remove-habbits" @click="removeHabbits = !removeHabbits">Remove Habbits</div>
    </div>
    <div v-if="storedName" class="cards-container">
      <Card v-for="(habbit, i) in habbits" :key="i" :habbit="habbit" :removeHabbits="removeHabbits" :index="i" @update-habbits="updateHabbits" @remove="removeHabbit"/>
      <div v-if="!addingHabbit" @click="addingHabbit = true" class="add-placeholder">
        Add<span>+</span>
      </div>
      <div v-else class="new-habbit">
        <input v-model="newHabbit" type="text" placeholder="name"/>
        <div class="colors">
          <div v-for="(color, i) in colors" :key="i" class="color" @click="selectColor(i)" :style="{border: `2px solid ${color}`, background: color ===  selectedColor ? color : ''}"></div>
        </div>
        <button class="add-habbit" @click="addHabbit()">Add habbit</button>
      </div>
    </div>

    <div v-if="!storedName" class="enter-name">
      <input v-model="enteredName" type="text" placeholder="name"/>
      <button class="submit-name" @click="submitName()">Submit</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Card from './components/Card.vue'

export default {
  components: {
    Card
  },
  data() {
    return {
      habbits: [],
      newHabbits: [],
      addingHabbit: false,
      storedName: localStorage.getItem('name') ? localStorage.getItem('name') : false,
      enteredName: "",
      newHabbit: "",
      selectedColor: "",
      removeHabbits: false,
      colors: [
        "lightblue",
        "lightgreen",
        "pink",
        "orange",
        "mediumpurple",
        "rosybrown",
        "indianred",
        "gold"
      ]
    }
  },
  created() {
    this.habbits = localStorage.getItem('habbits') ? JSON.parse(localStorage.getItem('habbits')) : []
  },
  computed: {
      records: function() {
          return this.habbits[0].streak.length;
      },
  },
  methods: {
      updateHabbits(updatedHabbit) {
          let updatedHabbits = this.habbits;
          updatedHabbits[updatedHabbit.index] = updatedHabbit.newHabbit;
          this.habbits = updatedHabbits;
          localStorage.setItem('habbits', JSON.stringify(this.habbits));
      },
      submitName() {
        if (this.enteredName) {
          localStorage.setItem('name', this.enteredName);
          // get habbits
          console.log("OK");
          axios.get('https://jsonplaceholder.typicode.com/todos/1')
            .then(response => {
              console.log(response.data)
            })
        }
      },
      addHabbit() {
        if (!this.newHabbit || !this.selectedColor) return;
        let newHabbit = {};
        newHabbit["name"] = this.newHabbit;
        newHabbit["records"] = '0';
        newHabbit["color"] = this.selectedColor;
        this.habbits = [...this.habbits, newHabbit];
        this.selectedColor = '';
        this.enteredName = '';
        localStorage.setItem('habbits', JSON.stringify(this.habbits));
        this.addingHabbit = false;
      },
      removeHabbit(index) {
        let habbits = [];
        for (let i = 0; i < this.habbits.length; i++) {

          if (i !== index) {
            habbits.push(this.habbits[i]);
          }
        }
        this.habbits = habbits;
        localStorage.setItem('habbits', JSON.stringify(this.habbits));
      },
      selectColor(i) {
        this.selectedColor = this.colors[i];
      }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html, body {
  height: 100vh;
}

body {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
}

.habbitlog {
  margin: 10px;
  font-weight: bold;
  font-size: 30px;
  display: inline-block;
}

button, a {
  cursor: pointer;
}

button:focus {
  outline: none;
}
</style>

<style scoped>
.enter-name {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
}

.enter-name input {
    padding: 5px;
    font-size: 24px;
    text-align: center;
}

.submit-name {
    padding: 5px;
    font-size: 24px;
    background: #dd5f8b;
    color: white;
}

.color {
    width: 20%;
    height: 15px;
    margin: 2px;
    cursor: pointer;
}

.colors {
    display: flex;
    justify-content: center;
    width: 100%;
    flex-wrap: wrap;
    margin: 5px 0;
}

.habbit {
    display: flex;
    align-items: center;
    border-radius: 15px;
    box-shadow: 1px 3px 4px 0px lightgrey;
    width: fit-content;
    padding: 2px 10px;
    position: relative;
}

.name {
    font-size: 24px;
    margin-left: 5px;
}

.new-habbit {
    width: 150px;
    margin: 10px;
    margin-top: 38px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
    box-shadow: 2px 2px 10px -3px lightgrey;
    padding: 5px;
    border-radius: 5px;
}

.new-habbit input {
    border: 1px solid lightgrey;
    border-radius: 5px;
    padding: 2px;
    text-align: center;
}

.add-habbit {
    background: #3b3b3b;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 20px;
}

.add-placeholder {
    margin-top: 38px;
    width: 150px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 3px solid #f8f8f8;
    color: #dbdbdb;
    height: 50px;
    border-radius: 5px;
    font-size: 20px;
    cursor: pointer;
}

.add-placeholder span {
  font-size: 26px;
  margin-left: 5px;
}
</style>