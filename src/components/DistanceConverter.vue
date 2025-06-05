<template>
  <div class="calculator-section distance-converter">
    <h2>Konverter Jarak</h2>
    <div class="conversion-group">
      <input type="number" v-model.number="inputValue" @input="convertDistance" placeholder="Masukkan nilai">
      <select v-model="fromUnit" @change="convertDistance">
        <option value="km">Kilometer (km)</option>
        <option value="m">Meter (m)</option>
        <option value="cm">Centimeter (cm)</option>
        <option value="mi">Mil (mi)</option>
        <option value="ft">Kaki (ft)</option>
        <option value="in">Inci (in)</option>
      </select>
      <span>ke</span>
      <select v-model="toUnit" @change="convertDistance">
        <option value="km">Kilometer (km)</option>
        <option value="m">Meter (m)</option>
        <option value="cm">Centimeter (cm)</option>
        <option value="mi">Mil (mi)</option>
        <option value="ft">Kaki (ft)</option>
        <option value="in">Inci (in)</option>
      </select>
    </div>

    <div v-if="outputValue !== null" class="result">
      Hasil: {{ outputValue.toFixed(4) }} {{ unitLabel(toUnit) }}
    </div>
    <div v-if="conversionError" class="result error-message">
      {{ conversionError }}
    </div>
  </div>
</template>

<script>
const conversionFactorsToMeter = {
  km: 1000,
  m: 1,
  cm: 0.01,
  mi: 1609.34,
  ft: 0.3048,
  in: 0.0254
};

const unitLabels = {
  km: 'km',
  m: 'm',
  cm: 'cm',
  mi: 'mil',
  ft: 'kaki',
  in: 'inci'
};

export default {
  name: 'DistanceConverter',
  data() {
    return {
      inputValue: null,
      fromUnit: 'km',
      toUnit: 'mi',
      outputValue: null,
      conversionError: null
    };
  },
  methods: {
    unitLabel(unit) {
      return unitLabels[unit] || unit;
    },
    convertDistance() {
      this.outputValue = null;
      this.conversionError = null;

      if (this.inputValue === null || isNaN(this.inputValue)) {
         if (this.inputValue !== null) {
            this.conversionError = "Masukkan angka yang valid untuk jarak.";
         }
        return;
      }

      const value = parseFloat(this.inputValue);

      if (!conversionFactorsToMeter[this.fromUnit] || !conversionFactorsToMeter[this.toUnit]) {
        this.conversionError = "Unit tidak valid.";
        return;
      }

      // Konversi nilai input ke meter
      const valueInMeters = value * conversionFactorsToMeter[this.fromUnit];

      // Konversi dari meter ke unit tujuan
      this.outputValue = valueInMeters / conversionFactorsToMeter[this.toUnit];
    }
  },
  watch: {
    inputValue() { this.convertDistance(); },
    fromUnit() { this.convertDistance(); },
    toUnit() { this.convertDistance(); }
  }
};
</script>

<style scoped>
.distance-converter .conversion-group {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}
.distance-converter input[type="number"] {
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