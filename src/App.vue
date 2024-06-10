<template>
  <div class="card container text-center">
    <h1 class="card-header">Archery Score Recording Application</h1>
    <div class="card-body">
      <div v-if="!isSet">
        <setUpComponent ref="main" @dataSelected="handleDataSelected"></setUpComponent>
      </div>
      <div v-else>
        <div>
          <button @click="backToMain" type="button" class="btn btn-outline-danger">Home</button>
        </div>
        <h3 class="m-2">{{ submittedArcher.firstName }} {{ submittedArcher.lastName }}, {{
          submittedArcher.equipment }}</h3>
        <div class="range-navigation m-2">
          <button class="btn btn-outline-warning btn-sm m-2" @click="decrementRange"
            :disabled="selectedRange === 0">Prev</button>
          <span class="range-count">{{ selectedRange + 1 }} / {{ submittedRound.ranges.length }}</span>
          <button class="btn btn-outline-warning btn-sm m-2" @click="incrementRange"
            :disabled="selectedRange === submittedRound.ranges.length - 1">Next</button>
        </div>
        <div class="card p-2 m-2">
          <rangeComponent ref="rangeComponents" v-for="(range, index) in submittedRound.ranges" :key="index"
            :range="range" v-show="index === selectedRange"></rangeComponent>
        </div>
        <div>
          <button class="btn btn-outline-dark m-2" @click="exportJSON">Export JSON file for result</button>
        </div>
        <div class="alert alert-warning" v-if="msg !== ''">
          {{ msg }}
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import setUpComponent from '@/components/setUpComponent.vue'
import rangeComponent from '@/components/rangeComponent.vue'

export default {
  name: 'App',
  components: {
    setUpComponent,
    rangeComponent
  },
  data() {
    return {
      isSet: false,
      submittedArcher: {},
      submittedRound: {},
      selectedRange: 0,
      msg: ''
    };
  },
  methods: {
    handleDataSelected(data) {
      this.isSet = true;
      this.submittedArcher = data.archer;
      this.submittedRound = data.round;
    },
    incrementRange() {
      if (this.selectedRange < this.submittedRound.ranges.length - 1) {
        this.selectedRange++;
      }
    },
    decrementRange() {
      if (this.selectedRange > 0) {
        this.selectedRange--;
      }
    },
    exportJSON() {
      if (this.canExportJSON) {
        const rangeRecords = [];
        for (const rangeComponent of Object.values(this.$refs.rangeComponents)) {
          if (rangeComponent.isSubmitted) {
            rangeRecords.push(rangeComponent.rangeRecord);
          }
        }
        const jsonData = {
          archerName: `${this.submittedArcher.firstName} ${this.submittedArcher.lastName}`,
          equipment: this.submittedArcher.equipment,
          roundShot: this.submittedRound.roundName,
          rangeRecords: rangeRecords
        }
        const jsonStr = JSON.stringify(jsonData, null, 2);
        const blob = new Blob([jsonStr], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = this.submittedArcher.firstName + "_" + this.submittedArcher.lastName + "_" + this.submittedRound.roundName + '.json';
        link.click();
        URL.revokeObjectURL(url);
      } else {
        this.msg = "At least one Range Record has to be submitted to export JSON file!"
      }
    },
    backToMain() {
      this.isSet = false;
      this.submittedArcher = {};
      this.submittedRound = {};
      this.selectedRange = 0;
      this.msg = '';
      // Reset range components
      for (const rangeComponent of Object.values(this.$refs.rangeComponents)) {
        if (rangeComponent.isSubmitted) {
          rangeComponent.resetRange();
        }
      }
    }
  },
  computed: {
    canExportJSON() {
      return this.$refs.rangeComponents.some(rangeComponent => {
        return rangeComponent.isSubmitted === true;
      })
    }
  }
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
