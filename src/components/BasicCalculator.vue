<template>
  <div class="calculator-section basic-calculator">
    <h2>Kalkulator Dasar</h2>
    <div class="input-group">
      <label for="num1">Angka 1:</label>
      <input type="number" id="num1" v-model.number="number1" @input="validateInput('number1')">
      <span v-if="errors.number1" class="error-message">{{ errors.number1 }}</span>
    </div>
    <div class="input-group">
      <label for="operator">Operator:</label>
      <select id="operator" v-model="operator">
        <option value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
      </select>
    </div>
    <div class="input-group">
      <label for="num2">Angka 2:</label>
      <input type="number" id="num2" v-model.number="number2" @input="validateInput('number2')">
      <span v-if="errors.number2" class="error-message">{{ errors.number2 }}</span>
    </div>
    <button @click="calculate" class="calculate-btn" :disabled="!isValid">Hitung</button>

    <div v-if="result !== null" class="result">
      Hasil: {{ result }}
    </div>
    <div v-if="calculationError" class="result error-message">
      {{ calculationError }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'BasicCalculator',
  data() {
    return {
      number1: null,
      number2: null,
      operator: '+',
      result: null,
      calculationError: null,
      errors: {
        number1: null,
        number2: null
      }
    };
  },
  computed: {
    isValid() {
      return this.number1 !== null && this.number2 !== null && this.errors.number1 === null && this.errors.number2 === null;
    }
  },
  methods: {
    validateInput(field) {
      if (this[field] === null || isNaN(this[field])) {
        this.errors[field] = 'Harap masukkan angka yang valid.';
      } else {
        this.errors[field] = null;
      }
      this.result = null; // Reset hasil jika input berubah
      this.calculationError = null;
    },
    calculate() {
      this.calculationError = null;
      this.result = null;

      if (!this.isValid) {
          this.calculationError = "Input tidak valid.";
          return;
      }

      let num1 = parseFloat(this.number1);
      let num2 = parseFloat(this.number2);

      switch (this.operator) {
        case '+':
          this.result = num1 + num2;
          break;
        case '-':
          this.result = num1 - num2;
          break;
        case '*':
          this.result = num1 * num2;
          break;
        case '/':
          if (num2 === 0) {
            this.calculationError = 'Tidak bisa membagi dengan nol!';
            this.result = null;
          } else {
            this.result = num1 / num2;
          }
          break;
        default:
          this.calculationError = 'Operator tidak valid.';
          this.result = null;
      }
    }
  },
  watch: {
    // Reset hasil jika input atau operator berubah
    number1() { this.result = null; this.calculationError = null; this.validateInput('number1'); },
    number2() { this.result = null; this.calculationError = null; this.validateInput('number2'); },
    operator() { this.result = null; this.calculationError = null; }
  }
};
</script>

<style scoped>
.basic-calculator .input-group {
  margin-bottom: 10px;
  color : black;
}
.basic-calculator .input-group label {
    display: block;
    margin-bottom: 5px;
}
.basic-calculator .input-group input[type="number"],
.basic-calculator .input-group select {
  color: black;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
}
</style>