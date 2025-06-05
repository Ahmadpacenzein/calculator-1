<template>
  <div class="calculator-section ideal-weight-calculator">
    <h2>Kalkulator Berat Badan Ideal (Rumus Broca)</h2>
    <div class="input-group">
      <label for="height">Tinggi Badan (cm):</label>
      <input type="number" id="height" v-model.number="height" @input="validateHeight">
      <span v-if="errors.height" class="error-message">{{ errors.height }}</span>
    </div>
    <div class="input-group">
      <label for="gender">Jenis Kelamin:</label>
      <select id="gender" v-model="gender">
        <option value="male">Pria</option>
        <option value="female">Wanita</option>
      </select>
    </div>
    <button @click="calculateIdealWeight" class="calculate-btn" :disabled="!isHeightValid">Hitung BBI</button>

    <div v-if="idealWeight !== null" class="result">
      Berat Badan Ideal Anda: {{ idealWeight.toFixed(2) }} kg
    </div>
     <div v-if="calculationError" class="result error-message">
      {{ calculationError }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'IdealWeightCalculator',
  data() {
    return {
      height: null,
      gender: 'male',
      idealWeight: null,
      calculationError: null,
      errors: {
        height: null
      }
    };
  },
  computed: {
    isHeightValid() {
      return this.height !== null && this.height > 0 && this.errors.height === null;
    }
  },
  methods: {
    validateHeight() {
      if (this.height === null || isNaN(this.height) || this.height <= 0) {
        this.errors.height = 'Tinggi badan harus angka positif.';
      } else {
        this.errors.height = null;
      }
      this.idealWeight = null; // Reset hasil jika input berubah
      this.calculationError = null;
    },
    calculateIdealWeight() {
      this.calculationError = null;
      this.idealWeight = null;

      if (!this.isHeightValid) {
        this.calculationError = "Masukkan tinggi badan yang valid.";
        return;
      }

      const h = parseFloat(this.height);
      let bbi;

      if (this.gender === 'male') {
        bbi = (h - 100) - ((h - 100) * 0.10);
      } else { // female
        bbi = (h - 100) - ((h - 100) * 0.15);
      }

      if (bbi <= 0) {
          this.calculationError = "Tinggi badan terlalu rendah untuk perhitungan ini.";
          this.idealWeight = null;
      } else {
          this.idealWeight = bbi;
      }
    }
  },
  watch: {
    height() { this.validateHeight(); },
    gender() {
        this.idealWeight = null;
        this.calculationError = null;
        if(this.isHeightValid) this.calculateIdealWeight(); // recalculate if height is already valid
    }
  }
};
</script>

<style scoped>
.ideal-weight-calculator .input-group {
  margin-bottom: 10px;
}
.ideal-weight-calculator .input-group label {
    display: block;
    margin-bottom: 5px;
}
.input-group input{
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    color: black;
}
.input-group #gender {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    color: black;
}
</style>