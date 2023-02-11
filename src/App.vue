<script>
import axios from "axios";

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
        loanPeriod: 0,
      },
      error: "",
    };
  },
  methods: {
    submit() {
        axios
          .post("http://localhost:8080/api/v1/calculate", this.form)
          .then(({ data }) => {
            this.error = "";
            this.result.amount = data?.amount;
            this.result.loanPeriod = data?.loanPeriod;
          })
          .catch(({response}) => {
            this.error = response?.data?.error;
          });
    },
  },
};
</script>

<template>
  <div class="calculator-box">
    <h2>Loan calculator</h2>
    <form @submit.prevent="submit">
      <div class="input-box">
        <input type="text" v-model="form.personalCode" required maxlength="11" minlength="11"/>
        <label>Personal code</label>
      </div>
      <div class="input-box">
        <input
          type="number"
          v-model="form.loanAmount"
          required
          min="2000"
          max="10000"
        />
        <label>Loan amount</label>
      </div>
      <div class="input-box">
        <input
          type="number"
          v-model="form.loanPeriod"
          required
          min="12"
          max="60"
        />
        <label>Loan period in months</label>
      </div>

      <button type="submit">Submit</button>

      <div class="message" v-if="!error && result.amount">
        <p>
          Possible to lend you an amount of {{ result.amount }} over
          {{ result.loanPeriod }} months.
        </p>
      </div>

      <div class="error-message" v-if="error">{{ error }}</div>
    </form>
  </div>
</template>

<style>
html {
  height: 100%;
}

body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  background: linear-gradient(#5d3891, #2b0055);
}

.calculator-box {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 400px;
  padding: 40px;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
}

.calculator-box h2 {
  margin: 0 0 30px;
  padding: 0;
  color: #fff;
  text-align: center;
}

.calculator-box .input-box {
  position: relative;
}

.calculator-box .input-box input {
  width: 100%;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  margin-bottom: 30px;
  border: none;
  border-bottom: 1px solid #fff;
  outline: none;
  background: transparent;
}

.calculator-box .input-box label {
  position: absolute;
  top: 0;
  left: 0;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  pointer-events: none;
  transition: 0.5s;
}

.calculator-box .input-box input:focus ~ label,
.calculator-box .input-box input:valid ~ label {
  top: -26px;
  left: 0;
  color: #5d3891;
}

button {
  padding: 10px 20px;
  color: #5d3891;
  font-size: 16px;
  text-decoration: none;
  text-transform: uppercase;
  overflow: hidden;
  transition: 0.5s;
  margin-top: 32px;
  letter-spacing: 4px;
  border: none;
  border-radius: 5px;
}

button:hover {
  background: #5d3891;
  color: #fff;
}

.message {
  color: #fff;
  margin-top: 32px;
  font-size: 18px;
}

.error-message {
  color: #f94a29;
  margin-top: 32px;
}
</style>
