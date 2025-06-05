<template>
  <div class="calculator-section temperature-converter">
    <h2>Konverter Suhu</h2>
    <div class="conversion-group">
      <input type="number" v-model.number="inputValue" @input="convertTemperature" placeholder="Masukkan nilai">
      <select v-model="fromUnit" @change="convertTemperature">
        <option value="C">Celcius (°C)</option>
        <option value="F">Fahrenheit (°F)</option>
        <option value="R">Reamur (°R)</option>
        <option value="K">Kelvin (K)</option>
      </select>
      <span>ke</span>
      <select v-model="toUnit" @change="convertTemperature">
        <option value="C">Celcius (°C)</option>
        <option value="F">Fahrenheit (°F)</option>
        <option value="R">Reamur (°R)</option>
        <option value="K">Kelvin (K)</option>
      </select>
    </div>

    <div v-if="outputValue !== null" class="result">
      Hasil: {{ outputValue.toFixed(2) }} °{{ toUnit }}
    </div>
     <div v-if="conversionError" class="result error-message">
      {{ conversionError }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'TemperatureConverter',
  data() {
    return {
      inputValue: null,
      fromUnit: 'C',
      toUnit: 'F',
      outputValue: null,
      conversionError: null
    };
  },
  methods: {
    convertTemperature() {
      this.outputValue = null; // Reset output
      this.conversionError = null;

      if (this.inputValue === null || isNaN(this.inputValue)) {
        if (this.inputValue !== null) { // Hanya tampilkan error jika ada input tapi tidak valid
            this.conversionError = "Masukkan angka yang valid untuk suhu.";
        }
        return;
      }

      const value = parseFloat(this.inputValue);
      let tempInCelcius;

      // Konversi unit input ke Celcius dulu
      switch (this.fromUnit) {
        case 'C':
          tempInCelcius = value;
          break;
        case 'F':
          tempInCelcius = (value - 32) * 5 / 9;
          break;
        case 'R':
          tempInCelcius = value * 5 / 4;
          break;
        case 'K':
          tempInCelcius = value - 273.15;
          break;
        default:
          this.conversionError = "Unit asal tidak valid.";
          return;
      }

      // Konversi dari Celcius ke unit tujuan
      switch (this.toUnit) {
        case 'C':
          this.outputValue = tempInCelcius;
          break;
        case 'F':
          this.outputValue = (tempInCelcius * 9 / 5) + 32;
          break;
        case 'R':
          this.outputValue = tempInCelcius * 4 / 5;
          break;
        case 'K':
          this.outputValue = tempInCelcius + 273.15;
          break;
        default:
          this.conversionError = "Unit tujuan tidak valid.";
          return;
      }
    }
  },
  watch: {
    inputValue() { this.convertTemperature(); },
    fromUnit() { this.convertTemperature(); },
    toUnit() { this.convertTemperature(); }
  }
};
</script>

<style scoped>
.temperature-converter .conversion-group {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}
.temperature-converter input[type="number"] {
  flex-grow: 1; 
}
.conversion-group input{
    border-color: #eaeaee;
    color: black;

}
.conversion-group select {
  padding: 5px;
  border: 1px solid #eaeaee;
  border-radius: 4px;
  color: black;
  border-color: black;
}
</style>