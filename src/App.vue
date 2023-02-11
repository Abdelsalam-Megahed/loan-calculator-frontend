<script>
import axios from "axios"

export default {
  data() {
    return {
      form: {
        personalCode: "",
        loanAmount: "",
        loanPeriod: "",
      },
      result: {
        amount: 0,
        loanPeriod: 0
      },
      error: ""
    };
  },
  methods: {
    submit() {
      axios
        .post("http://localhost:8080/api/v1/calculate", this.form)
        .then((response) => {
            return response.data;
        })
        .then((data) => {     
          this.error = ""
          this.result.amount = data?.amount
          this.result.loanPeriod = data?.loanPeriod
        })
        .catch((error) => {
          this.error = error?.response?.data?.error
        });
    },
  },
};
</script>

<template>
  <div>
    <h3>Loan calculator</h3>
    <form @submit.prevent="submit">
    <div>
      <span>Enter personal code</span>
      <input 
      type="text" 
      v-model="form.personalCode" 
      required />
    </div>
    <div>
      <span>Enter loan amount</span>
      <input
        type="number"
        v-model="form.loanAmount"
        required
        min="2000"
        max="10000"
      />
    </div>
    <div>
      <span>Enter loan period</span>
      <input
        type="number"
        v-model="form.loanPeriod"
        required
        min="12"
        max="60"
      />
    </div>

    <button type="submit">Submit</button>

    <div v-if="!error">
      <p>Possible to lend you an amount of {{ result.amount }} over {{ result.loanPeriod }} months.</p>
    </div>

    <div v-if="error">{{ error }}</div>
    </form>
  </div>
</template>

<style>
.center {
  margin: auto;
  width: 50%;
}

span {
  padding-right: 12px;
  padding-bottom: 12px;
}
</style>