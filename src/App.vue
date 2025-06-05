<template>
  <div id="app-container">
    <header>
      <h1>Kalkulator Multifungsi</h1>
      <nav>
        <button @click="activeCalculator = 'basic'" :class="{ active: activeCalculator === 'basic' }">Dasar</button>
        <button @click="activeCalculator = 'temperature'" :class="{ active: activeCalculator === 'temperature' }">Suhu</button>
        <button @click="activeCalculator = 'idealWeight'" :class="{ active: activeCalculator === 'idealWeight' }">Berat Ideal</button>
        <button @click="activeCalculator = 'distance'" :class="{ active: activeCalculator === 'distance' }">Jarak</button>
      </nav>
    </header>

    <main>
      <BasicCalculator v-if="activeCalculator === 'basic'" />
      <TemperatureConverter v-if="activeCalculator === 'temperature'" />
      <IdealWeightCalculator v-if="activeCalculator === 'idealWeight'" />
      <DistanceConverter v-if="activeCalculator === 'distance'" />
    </main>

    <footer>
      <p>&copy; 2025 by Ahmad Zaini</p>
    </footer>
  </div>
</template>

<script>
import BasicCalculator from './components/BasicCalculator.vue';
import TemperatureConverter from './components/TemperatureConverter.vue';
import IdealWeightCalculator from './components/IdealWeightCalculator.vue';
import DistanceConverter from './components/DistanceConverter.vue';

export default {
  name: 'App',
  components: {
    BasicCalculator,
    TemperatureConverter,
    IdealWeightCalculator,
    DistanceConverter
  },
  data() {
    return {
      activeCalculator: 'basic' // Kalkulator yang aktif secara default
    };
  }
};
</script>

<style>
/* Styling global atau spesifik untuk App.vue */
#app-container {
  width: 40rem;
  height: 50rem;
  margin: 40px auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-family: 'Arial', sans-serif;
  color: #333;
}

header {
  text-align: center;
  margin-bottom: 30px;
}

header h1 {
  color: #333;
  font-size: 2.5em;
}

nav {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 10px;
}

nav button {
  padding: 10px 20px;
  border: 1px solid #ccc;
  background-color: #fff;
  color: #555;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s, color 0.3s;
}

nav button:hover {
  background-color: #e0e0e0;
}

nav button.active {
  background-color: #fffacd; /* Kuning pucat */
  border-color: #f0e68c; /* Kuning pucat lebih gelap untuk border */
  color: #333;
  font-weight: bold;
}

main {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 5px;
  min-height: 300px;
}

footer {
  text-align: center;
  margin-top: 30px;
  font-size: 0.9em;
  color: #777;
}

/* Styling umum untuk input dan button di dalam komponen */
.calculator-section {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.calculator-section label {
  font-weight: bold;
  color: #444;
}

.calculator-section input[type="number"],
.calculator-section input[type="text"],
.calculator-section select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
  background-color: #f9f9f9; /* Abu-abu sangat muda untuk input */
}

.calculator-section input[type="number"]:focus,
.calculator-section input[type="text"]:focus,
.calculator-section select:focus {
  outline: none;
  border-color: #fffacd; /* Kuning pucat saat focus */
  box-shadow: 0 0 0 2px rgba(255, 250, 205, 0.5); /* Bayangan kuning pucat */
}


.calculator-section button.calculate-btn {
  padding: 12px 20px;
  background-color: #6c757d; /* Abu-abu gelap */
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s;
}

.calculator-section button.calculate-btn:hover {
  background-color: #5a6268; /* Abu-abu lebih gelap saat hover */
}

.result {
  margin-top: 20px;
  padding: 15px;
  background-color: #fffacd; /* Kuning pucat untuk hasil */
  border: 1px solid #f0e68c;
  border-radius: 4px;
  font-size: 1.2em;
  font-weight: bold;
  color: #333;
  text-align: center;
}

.error-message {
  color: red;
  font-size: 0.9em;
  margin-top: 5px;
}
</style>