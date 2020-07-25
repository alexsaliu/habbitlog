<template>
  <div id="app">
    <div class="habbitlog">HabbitLog</div>
    <div class="cards-container">
      <Card v-for="(habbit, i) in habbits" :key="i" :habbit="habbit" :index="i" @update-habbits="updateHabbits"/>
    </div>
  </div>
</template>

<script>
import Card from './components/Card.vue'

export default {
  components: {
    Card
  },
  data() {
    return {
      habbits: [],
      selectedHabbits: [

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
      }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html, body {
  height: 100%;
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
}
</style>
