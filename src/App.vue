<template>
  <div
    id="app"
    :class="
      typeof currentData.main != 'undefined' && currentData.main.temp <16
        ? 'cold'
        : ''
    "
  >
    <main>
      <div class="header">One Week Weather</div>
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

body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('./assets/warm-bg.jpg');
  background-size:cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.cold {
  background-image: url('./assets/cold-bg.jpg');
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
@import url('https://fonts.googleapis.com/css2?family=Gloria+Hallelujah&display=swap');
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-family: 'Gloria Hallelujah', cursive;
  color: white;
  font-weight:700;
  font-size: 70px;
}
</style>
