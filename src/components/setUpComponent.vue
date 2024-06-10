<template>
    <div>
        <div>
            <div class="dropdown p-3">
                <label for="round">Choose Round:</label>
                <select class="form-select" id="round" name="round" v-model="selectedRound">
                    <option v-for="round in rounds" :key="round" :value="round">{{ round.roundName }}</option>
                </select>
            </div>
            <div class="dropdown p-3">
                <label for="archer">Choose Archer:</label>
                <select class="form-select" id="archer" name="archer" v-model="selectedArcher">
                    <option v-for="archer in archers" :key="archer" :value="archer">
                        {{ archer.firstName }} {{ archer.lastName }}
                    </option>
                </select>
            </div>
        </div>
        <div class="table-responsive" v-if="selectedArcher !== ''">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Equipment</th>
                        <th>Selected</th>
                    </tr>
                </thead>
                <tbody class="table-group-divider">
                    <tr>
                        <td>{{ selectedArcher.firstName }}</td>
                        <td>{{ selectedArcher.lastName }}</td>
                        <td>
                            <select class="form-select" v-model="selectedArcher.defaultEquipment">
                                <option v-for="equipment in equipmentOptions" :key="equipment" :value="equipment">{{
                                    equipment }}</option>
                            </select>
                        </td>
                        <td>
                            <button type="button" class="btn btn-outline-danger" @click="remove">Remove</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div v-if="msg !== '' && !validData" class="alert alert-warning" role="alert">
            {{ msg }}
        </div>
        <button @click="handleReturn" class="btn btn-outline-primary">Done</button>
    </div>
</template>
  
<script>

export default {
    name: 'setUp',
    data() {
        return {
            rounds: [],
            selectedRound: '', // Initially no round selected
            archers: [],
            selectedArcher: '', // Initially no archer selected
            equipmentOptions: ['Recurve', 'Compound', 'Longbow', 'Recurve Barebow', 'Compound Barebow'],
            msg: ''
        };
    },
    created() {
        var self = this;
        try {
            self.rounds = require("@/assets/json/rounds.json");
        } catch (error) {
            self.msg = "Errors in getting the data: " + error.message;
        }
        try {
            self.archers = require("@/assets/json/archers.json");
        } catch (error) {
            self.msg = "Errors in getting the data: " + error.message;
        }
    },
    methods: {
        remove() {
            this.selectedArcher = '';
        },
        handleReturn() {
            if (this.validData) {
                this.$emit('dataSelected', {
                    round: this.selectedRound,
                    archer: {
                        firstName: this.selectedArcher.firstName,
                        lastName: this.selectedArcher.lastName,
                        equipment: this.selectedArcher.defaultEquipment
                    }
                });
            } else {
                this.msg = "Please fill out the information!"
            }
        },
        reset() {
            this.selectedRound = '';
            this.selectedArcher = '';
            this.msg = '';
        }
    },
    computed: {
        validData() {
            return this.selectedArcher !== '' && this.selectedRound !== '';
        }
    }
};
</script>
<style>
th {
    text-align: center;
}
</style>