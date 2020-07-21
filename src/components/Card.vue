<template>
  <div class="card">
    <div class="info">
      <div class="title">{{index + 1}}.</div>
      <div class="score">{{score}}/{{habbits.length}}</div>
    </div>
    <div class="item-container">
      <div
        v-for="(habbit, i) in habbits"
        :key="i"
        :style="{background: habbit.streak[index] === '1' ? habbit.color : '#fbfbfb'}"
        :class="['item', {complete: score === habbits.length}]"
        @click="changeStatus(i)"
      >
        {{habbit.name}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    habbits: Array,
    index: Number
  },
  data() {
    return {
      goals: ["bed 10pm", "read 15", "exercise", "1 hour", "meditate", "nfp"],
      months: ["Jan", "Feb", "Mar", "Apr", "May", "June",
        "July", "Aug", "Sep", "Oct", "Nov", "Dec"
      ]
    }
  },
  computed: {
    score: function() {
      let score = 0;
      this.habbits.forEach(habbit => {
          console.log("index: ", habbit.streak);
        if (habbit.streak[this.index] === '1') score++;
      })
      return score;
    },
    // date: function() {
    //   const timestamp = this.record.timestamp;
    //   const date = new Date();
    //   console.log(new Date(timestamp * 1000));
    //
    //   return date.getDate() + " " + this.months[date.getMonth()] + " " +  date.getFullYear();
    // }
  },
  methods: {
    changeStatus(i) {
        let updatedHabbits = [...this.habbits];
        let streak = updatedHabbits[i].streak;
        let status = streak[this.index];
        let newStatus = status === '0' ? '1' : '0';
        let newStreak = this.alterString(streak, this.index, newStatus);
        updatedHabbits[i].streak = newStreak;
        this.$emit('update-habbits', updatedHabbits);
    },
    alterString(oldString, index, replacement) {
      return oldString.substr(0, index) + replacement + oldString.substr(index + replacement.length);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
    margin: 10px;
}

.info {
  font-size: 10px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  margin: 2px 5px;
}

.item-container {
  width: 152px;
  display: flex;
  flex-wrap: wrap;
  font-size: 10px;
  box-shadow: 2px 2px 10px -3px lightgrey;
  border-radius: 5px;
}

.item {
  width: 46px;
  height: 46px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  margin: 2px;
  cursor: pointer;
  background: #fbfbfb;
  padding: 2px;
  word-break: break-word;
}

.done {
  background: #ffed8f;
}

.complete {
  background: #c0fd81;
}
</style>
