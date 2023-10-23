<script>
import axios from 'axios';

export default {
  data() {
    return {
      cities: [],
      branches: [],
      selectedCity: "",
      selectedBranch: ""
    };
  },
  methods: {
    async fetchCities() {
      try {
        const apiUrl = 'https://api.novaposhta.ua/v2.0/json/';
        const requestData = {
          apiKey: '2dc5ec82c3f1fa7bf6bce3f106ad167c', 
          modelName: 'Address',
          calledMethod: 'getCities',
          methodProperties: {}
        };

        const response = await axios.post(apiUrl, requestData);

        this.cities = response.data.data;
      } catch (error) {
        console.error('Помилка при запиті до API Нової Пошти:', error);
      }
    },
    async fetchBranches() {
      try {
        const apiUrl = 'https://api.novaposhta.ua/v2.0/json/';
        const requestData = {
          apiKey: '2dc5ec82c3f1fa7bf6bce3f106ad167c', 
          modelName: 'AddressGeneral',
          calledMethod: 'getWarehouses',
          methodProperties: {
            CityRef: this.selectedCity
          }
        };

        const response = await axios.post(apiUrl, requestData);

        this.branches = response.data.data;
      } catch (error) {
        console.error('Помилка при запиті до API Нової Пошти:', error);
      }
    },
  },
  created() {
    this.fetchCities();
  }
};
</script>

<template>
  <div class="container">
    <div class="input-container">
      <label for="city" class="label">Місто:</label>
      <select v-model="selectedCity" @change="fetchBranches" class="select">
        <option v-for="city in cities" :key="city.Ref" :value="city.Ref" class="option">{{ city.Description }}</option>
      </select>
    </div>

    <div class="input-container">
      <label for="branch" class="label">Поштове відділення:</label>
      <select v-model="selectedBranch" class="select">
        <option v-for="branch in branches" :key="branch.Ref" :value="branch.Ref" class="option">{{ branch.Description }}</option>
      </select>
    </div>
  </div>
</template>

<style scoped>
body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 600px;
  margin: 0;
}

.container {
  width: 350px;
  font-family: 'Montserrat', sans-serif;
  text-align: center;
}

.input-container {
  margin-bottom: 20px;
  text-align: left;
}

.label {
  font-weight: bold;
  display: block;
}

.select {
  width: 100%;
  padding: 10px;
  border: 1px solid #222222;
  border-radius: 5px;
  /* background-color: white; */
}
</style>
