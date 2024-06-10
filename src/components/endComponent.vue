<template>
  <div>
    <div class="m-4">
      <h4>End {{ endNumber }}</h4>
      <button class="btn btn-outline-primary" @click="toggleKeypad" :disabled="isSubmitted">Edit End</button>
    </div>
    <div class="container m-4">
      <div class="row">
        <div class="col" v-for="index in 6" :key="index">
          <input type="text" class="form-control square-input" v-model="scores[index - 1]" readonly />
        </div>
      </div>
    </div>
    <div class="m-4">
      Total: {{ totalScore }}
    </div>
    <div class="alert alert-success" role="alert" v-if="msg !== ''">
      {{ msg }}
    </div>
    <div class="alert alert-warning" role="alert" v-if="errMsg !== ''">
      {{ errMsg }}
    </div>
    <div>
      <button type="button" class="btn btn-outline-primary mr-2" @click="resetScore">Reset</button>
      <button type="button" class="btn btn-outline-primary ml-2" @click="submitScore"
        :disabled="!scoresCompleted || isSubmitted">Submit</button>
    </div>
    <div>
      <keypadComponent v-if="showKeypad && !scoresCompleted && !isSubmitted" @score-selected="updateScore">
      </keypadComponent>
    </div>
  </div>
</template>
  
<script>
import keypadComponent from './keypadComponent.vue'

export default {
  name: 'endComponent',
  components: {
    keypadComponent
  },
  props: {
    endNumber: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      scores: ['', '', '', '', '', ''],
      showKeypad: false,
      writtenIndex: 0,
      msg: '',
      errMsg: '',
      isSubmitted: false
    }
  },
  methods: {
    toggleKeypad() {
      // console.log("Toggle keypad of" + " End " + this.endNumber)
      if (this.showKeypad === false) {
        this.showKeypad = true;
      } else {
        this.showKeypad = false;
      }
    },
    submitScore() {
      if (this.validatedScore) {
        this.isSubmitted = true;
        this.$emit('scores-submitted', this.endRecord);
        this.msg = "The scores have been submitted. If you wish to make changes, please click reset."
      } else {
        this.errMsg = "Please enter the scores in descending order. Reset score to enter the scores again.";
      }
    },
    resetScore() {
      console.log("The scores are resetted in End: " + this.endNumber);
      this.showKeypad = false;
      this.scores = ['', '', '', '', '', ''];
      this.writtenIndex = 0;
      this.msg = '';
      if (this.isSubmitted) {
        this.isSubmitted = false;
        this.$emit('scores-reset', this.endNumber);
      }
    },
    updateScore(score) {
      this.scores[this.writtenIndex++] = score;
    }
  },
  computed: {
    scoresCompleted() {
      return this.scores.every(score => score !== '');
    },
    validatedScore() {
      const scores = this.scores.map(score => {
        if (score === 'X') {
          return 11;
        } else if (score === 'M') {
          return 0;
        } else {
          return parseInt(score);
        }
      });
      for (let i = 1; i < scores.length; i++) {
        if (scores[i] > scores[i - 1]) {
          return false;
        }
      }
      return true;
    },
    endRecord() {
      return {
        endNumber: this.endNumber,
        scores: this.scores
      }
    },
    totalScore() {
      let totalScore = 0;
      if (this.scores[0] !== '') {
        const scores = this.scores.map(score => {
          if (score === 'X') {
            return 10;
          } else if (score === 'M') {
            return 0;
          } else if (score === '') {
            return 0;
          } else {
            return parseInt(score);
          }
        });
        for (let i = 0; i < scores.length; i++) {
          totalScore += scores[i];
        }
      }
      return totalScore;
    }
  }
}
</script>
  
  
<style scoped>
.square-input {
  width: 50px;
  height: 50px;
}
</style>
  