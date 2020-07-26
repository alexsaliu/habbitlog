<template>
  <div id="app">
    <div class="habbitlog">HabbitLog</div>
    <div v-if="storedName" class="cards-container">
      <Card v-for="(habbit, i) in habbits" :key="i" :habbit="habbit" :index="i" @update-habbits="updateHabbits"/>
    </div>
    <div class="enter-name">
      <input v-model="enteredName" type="text" placeholder="name"/>
      <button class="submit-name" @click="submitName()">Submit</button>
    </div>
    <div class="select-habbits">
      <input v-model="newHabbit" type="text" placeholder="name"/>
      <button class="submit-name" @click="addHabbit()">Submit</button>
      <div class="colors">
        <div v-for="(color, i) in colors" :key="i" class="color" @click="selectColor(i)" :style="{border: `2px solid ${color}`, background: color ===  selectedColor ? color : ''}"></div>
      </div>
      <div class="new-habbits">
        <div v-for="(habbit, i) in newHabbits" :key="i" class="habbit">
          <div class="new-habbit-color" :style="{background: habbit.color}"></div>
          <div class="name">{{habbit.name}}</div>
          <div class="remove" @click="removeHabbit(i)">x</div>
        </div>
      </div>
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
      storedName: localStorage.getItem('name') ? localStorage.getItem('name') : false,
      enteredName: "",
      newHabbit: "",
      selectedColor: "",
      colors: [
        "lightblue",
        "lightgreen",
        "pink"
      ]
    }
  },
  created() {
    this.habbits = localStorage.getItem('habbits') ? JSON.parse(localStorage.getItem('habbits')) : 
    [
      {
        name: 'bed 10pm',
        records: '010',
        color: 'lightgreen'
      },
      {
        name: 'exercise',
        records: '111',
        color: 'yellow'
      },
      {
        name: '1 hour',
        records: '111',
        color: 'lightblue'
      }
    ]
  },
  computed: {
      records: function() {
          return this.habbits[0].streak.length;
      }
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
          // get habbits
          console.log("OK");
          axios.get('https://jsonplaceholder.typicode.com/todos/1')
            .then(response => {
              console.log(response.data)
            })
        }
      },
      addHabbit() {
        let newHabbit = {};
        newHabbit["name"] = this.newHabbit;
        newHabbit["color"] = this.selectedColor;
        this.newHabbits = [...this.newHabbits, newHabbit];
        this.selectedColor = '';
      },
      removeHabbit(index) {
        let habbits = [];
        for (let i = 0; i < this.newHabbits.length; i++) {

          if (i !== index) {
            habbits.push(this.newHabbits[i]);
          }
        }
        this.newHabbits = habbits;
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
    width: 40px;
    height: 40px;
    margin: 5px;
    cursor: pointer;
}

.colors {
    display: flex;
    justify-content: center;
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

.remove {
    font-size: 12px;
    color: white;
    background: #d93232;
    border-radius: 100px;
    padding: 0 1px 1px 0;
    height: 15px;
    width: 15px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    right: -8px;
    top: -5px;
    cursor: pointer;
}

.new-habbit-color {
    height: 20px;
    width: 20px;
    border-radius: 100px;
}
</style>