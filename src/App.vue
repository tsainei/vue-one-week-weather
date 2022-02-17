<template>
  <div
    id="app"
    :class="
      typeof currentData.main != 'undefined' && currentData.main.temp > 16
        ? 'warm'
        : ''
    "
  >
    <main>
      <div class="header">7 Day Weather App</div>
      <Search
        @showCards="showCard = true"
        @current="getCurrent"
        @daily="getDaily"
      />
      <div class="container">
        <Cards :current="currentData" :daily="dailyData" v-if="showCard" />
      </div>
    </main>
  </div>
</template>

<script>
import Cards from './components/cards.vue';
import Search from './components/search.vue';

export default {
  name: 'app',
  components: { Cards, Search },
  data() {
    return {
      currentData: [],
      dailyData: [],
      showCard: true,
    };
  },
  methods: {
    getCurrent(data) {
      this.currentData = data;
    },
    getDaily(data) {
      this.dailyData = data;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
@font-face {
  font-family: 'AlphaClouds';
  src: local('AlphaClouds'), url(./assets/AlphaClouds.ttf) format('truetype');
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
}
.container {
  height: fit-content;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-family: 'AlphaClouds';
  color: white;
  font-size: 70px;
}
</style>
