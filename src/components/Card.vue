<template>
  <div class="card">
    <div class="title">{{habbit.name}}</div>
    <div class="card-container" :style="{borderTop: `3px solid ${habbit.color}`}">
      <div v-if="removeHabbits" @click="$emit('remove', index)" class="remove">x</div>
      <div class="info">
        <div class="streak">streak <span>{{currentStreak}}</span></div>
        <div class="score">total {{habbit.records.length}}</div>
      </div>
      <div class="item-container">
        <div
          v-for="(status, i) in habbit.records"
          :key="i"
          :style="{background: status === '1' ? habbit.color : '#fbfbfb'}"
          :class="['item']"
          @click="changeStatus(status, i)"
        >
        </div>
      </div>
      <div class="add" @click="addItem">
        Add +
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    habbit: Object,
    index: Number,
    removeHabbits: Boolean
  },
  data() {
    return {
      months: ["Jan", "Feb", "Mar", "Apr", "May", "June",
        "July", "Aug", "Sep", "Oct", "Nov", "Dec"
      ]
    }
  },
  computed: {
    bestStreak: function() {
      const data = this.habbit.records;
      let bestStreak = 0;
      let streak = 0;
      for (let i = 1; i < data.length; i++) {
        if (data[i] === '1' && data[i] === data[i -1]) streak ++;
        if (streak > bestStreak) bestStreak = streak;
      }
      return data.includes('1') ? bestStreak + 1 : 0;
    },
    currentStreak: function() {
      const data = this.habbit.records;
      let streak = parseInt(data[data.length - 1]);
      for (let i = data.length - 1; i > 0; i--) {
        if (data[i - 1] === '0') return streak;
        streak ++;
      }
      return streak;
    }
  },
  methods: {
    changeStatus(status, i) {
        let newHabbit = this.habbit
        let newStatus = status === '1' ? '0' : '1';
        newHabbit.records = this.alterString(this.habbit.records, i, newStatus);
        this.$emit('update-habbits', {'index' : this.index, newHabbit});
    },
    alterString(oldString, index, replacement) {
      return oldString.substr(0, index) + replacement + oldString.substr(index + replacement.length);
    },
    addItem() {
      this.habbit.records = this.habbit.records + '0'
    },
    updateData() {

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
    color: grey;
    padding: 5px;
}

.card {
    margin: 10px;
}

.info {
  font-size: 11px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  margin: 5px;
}

.card-container {
  background: white;
  border-radius: 5px;
  box-shadow: 2px 2px 10px -3px lightgrey;
  padding: 2px;
  position: relative;
}

.item-container {
  width: 150px;
  font-size: 11px;
  position: relative;
  display: inline-block;
}

.item {
  height: 15px;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  margin: 4px;
  cursor: pointer;
  background: #fbfbfb;
  padding: 2px;
  word-break: break-word;
}

.add {
    font-size: 11px;
    padding: 2px;
    border: 2px solid #f8f8f8;
    border-radius: 5px;
    margin: 0px 5px 2px;
    cursor: pointer;
}

.remove {
    background: red;
    color: white;
    width: 16px;
    height: 16px;
    border-radius: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 11px;
    font-weight: bold;
    padding-bottom: 2px;
    position: absolute;
    right: -8px;
    top: -12px;
    cursor: pointer;
}
</style>
