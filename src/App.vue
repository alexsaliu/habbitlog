<template>
  <div id="app">
    <div>Name</div>
    <div class="cards-container">
      <Card v-for="(value, i) in records" :key="i" :habbits="habbits" :index="i" :update-habbits="updateHabbits"/>
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
    this.habbits = [
      {
        name: 'bed 10pm',
        streak: '010',
        color: 'lightgreen'
      },
      {
        name: 'exercise',
        streak: '111',
        color: 'yellow'
      },
      {
        name: '1 hour',
        streak: '111',
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
      updateHabbits(updatedHabbits) {
          this.habbits = updatedHabbits;
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

.body {
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
</style>
