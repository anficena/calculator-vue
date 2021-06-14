<template>
    <div id="app">
        <b-container>
            <b-alert show>CALCULATOR</b-alert>

            <b-row v-for="index in 3" :key="index">
                <b-col md="11">
                    <b-form-input
                        id="input-1"
                        type="number"
                        v-model="operand[index]"
                        placeholder="Enter number"
                        required
                    ></b-form-input>
                </b-col>
                <b-col md="1">
                    <b-form-checkbox
                        class="custom-cb"
                        id="checkbox-1"
                        v-model="checkbox[index]"
                        :value="index"
                        unchecked-value="not_accepted"
                    ></b-form-checkbox>
                </b-col>
            </b-row>
            <b-row>
                <b-col md="2" v-for="(btn, index) in btnOperator" :key="index">
                    <b-button class="operator" variant="outline-primary" @click="count(btn.value)">
                        <b-icon :icon="btn.icon" aria-hidden="true"></b-icon>
                    </b-button>
                </b-col>
            </b-row>
            <hr/>
            <b-alert v-model="showError" variant="danger">{{ errorMsg }}</b-alert>
            <b-row>
                <b-col>
                    Hasil: {{ result }}
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>

export default {
    name: "App",
    data() {
        return {
            operand: [],
            checkbox: [],
            btnOperator: [
                {
                    value: 'plus',
                    icon: 'plus'
                },
                {
                    value: 'minus',
                    icon: 'dash'
                },
                {
                    value: 'times',
                    icon: 'x'
                },
                {
                    value: 'divide',
                    icon: 'slash'
                }
            ],
            result: 0,
            showError: false,
            errorMsg: ""
        };
    },
    methods: {
        count(opt) { 
            let numbers = this.operand.filter((item, index) => this.checkbox.indexOf(index) > -1 );
            if(numbers.length < 2) {
                this.result = 0;
                this.showError = true;
                this.errorMsg = "Pastikan terdapat minimal dua angka yang akan di operasikan.";
            } else {
                this.result = this.operator(opt, numbers); 
            }
        },

        operator(opt, numbers) {
            const option = {
                plus: () => numbers.reduce((previousVal, currentVal) => parseFloat(previousVal) + parseFloat(currentVal)),
                minus: () => numbers.reduce((previousVal, currentVal) => parseFloat(previousVal) - parseFloat(currentVal)),
                times: () => numbers.reduce((previousVal, currentVal) => parseFloat(previousVal) * parseFloat(currentVal)),
                divide: () => numbers.reduce((previousVal, currentVal) => parseFloat(previousVal) / parseFloat(currentVal)),
            },
            result =  option[opt]();

            return result;
        }
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.row {
    margin-top: 20px !important;
}

.custom-cb input[type="checkbox"] {
    width: 35px;
    height: 35px;
}

.operator {
    width: 100%;
}

</style>
