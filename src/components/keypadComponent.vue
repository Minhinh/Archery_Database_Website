<template>
  <div class="keypad">
    <div class="row">
      <button class="pastel-red" @click="selectScore('X')">X</button>
      <button class="pastel-orange" @click="selectScore(10)">10</button>
      <button class="pastel-yellow" @click="selectScore(9)">9</button>
      <button class="pastel-green" @click="selectScore(8)">8</button>
    </div>
    <div class="row">
      <button class="pastel-blue" @click="selectScore(7)">7</button>
      <button class="pastel-indigo" @click="selectScore(6)">6</button>
      <button class="pastel-purple" @click="selectScore(5)">5</button>
      <button class="pastel-pink" @click="selectScore(4)">4</button>
    </div>
    <div class="row">
      <button class="pastel-coral" @click="selectScore(3)">3</button>
      <button class="pastel-lavender" @click="selectScore(2)">2</button>
      <button class="pastel-mint" @click="selectScore(1)">1</button>
      <button class="pastel-gray" @click="selectScore('M')">M</button>
    </div>
  </div>
</template>

<style>
.pastel-red {
  background-color: #FFC3C3;
}

.pastel-orange {
  background-color: #FFD8B8;
}

.pastel-yellow {
  background-color: #FFE8B3;
}

.pastel-green {
  background-color: #D8FFB8;
}

.pastel-blue {
  background-color: #B8E8FF;
}

.pastel-indigo {
  background-color: #B8C4FF;
}

.pastel-purple {
  background-color: #DAC4FF;
}

.pastel-pink {
  background-color: #FFB8D6;
}

.pastel-coral {
  background-color: #FFB8B8;
}

.pastel-lavender {
  background-color: #E8B8FF;
}

.pastel-mint {
  background-color: #B8FFD8;
}

.pastel-gray {
  background-color: #ECECEC;
}

.keypad .row {
  display: flex;
  justify-content: center;
}

.keypad button {
  width: 60px;
  height: 60px;
  margin: 5px;
  border-radius: 5px;
  border: none;
  font-size: 16px;
  color: #FFF;
  cursor: pointer;
}

</style>


<script>
export default {
  name: 'keypadComponent',
  methods: {
    selectScore(score) {
      // console.log("You have just click " + score)
      this.$emit('score-selected', score);
    }
  }
};
</script>