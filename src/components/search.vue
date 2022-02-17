<template>
    <div class="input-group mb-3 flex-nowrap search-box">
      <span class="input-group-text">
        <img src="../assets/search.png" alt
      /></span>
      <div class="search-box ms-0">
        <v-select
          type="text"
          class="search box search-bar ms-0"
          placeholder="Search for a City"
          v-model="selectedCity"
          @input="weatherLocation()"
          @search="onSearch"
          :options="cities"
          label="name"
          :filterable="false"
        >
          <template slot="no-options">Type City Name</template>
          <template slot="option" slot-scope="option">
            <div class="d-center">{{ option.name }}, {{ option.country }}</div>
          </template>
          <template slot="selected-option" slot-scope="option">
            <div class="selected d-center">
              <div class="d-center">
                {{ option.name }}, {{ option.country }}
              </div>
            </div>
          </template>
        </v-select>
      </div>
      <button class="input-group-text border-0 ms-0">
        <img src="../assets/target.png" alt />
      </button>
    </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      selectedCity: '',
      cities: [],
      lat: '',
      lon: '',
      api_key: '32c6ef671a0cb8362994c35ad6236b73',
      api_geo: 'https://api.openweathermap.org/geo/1.0/direct?',
      api_daily: 'https://api.openweathermap.org/data/2.5/onecall?',
      api_current: 'https://api.openweathermap.org/data/2.5/weather?',
    };
  },
  methods: {
    //search for city name
    onSearch(search, loading) {
      this.cities = [];
      if (search.length) {
        loading(true);
        this.search(loading, search, this);
      }
    },
    search(loading, search) {
      axios
        .get(`${this.api_geo}q=${search}&limit=5&appid=${this.api_key}`)
        .then((response) => {
          loading(false);
          this.cities = response.data;
        });
    },
    //select the city
    async weatherLocation() {
      this.lat = this.selectedCity.lat;
      console.log(this.selectedCity.name);
      this.lon = this.selectedCity.lon;
      await axios(
        `${this.api_daily}lat=${this.lat}&lon=${this.lon}&exclude=alerts,hourly,minutely,alerts,current&units=metric&appid=${this.api_key}`
      ).then((response) => {
        this.$emit('daily', response.data);
      });
      await axios(
        `${this.api_current}q=${this.selectedCity.name}&units=metric&appid=${this.api_key} `
      ).then((response) => {
        this.$emit('current', response.data);
        console.log(response.data);
      });
      this.$emit('showCards');
    },
  },
};
</script>

<style>
.search-box {
  width: 100%;
  margin-bottom: 30px;
  margin-left: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
}
.search-box .search-bar {
  border: none;
  width: 100%;
  padding: 15px;
  height: 50px;
  color: #313131;
  appearance: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}
.input-group .input-group-text {
  background-color: rgba(255, 255, 255, 0.5);
  height: 50px;
  border: none;
}
.v-select .vs__dropdown-toggle {
  border: none;
}
.v-select .vs__search {
  margin: 0;
}
.v-select .vs__dropdown-menu {
  background: rgba(255, 255, 255, 0.5);
}
</style>
