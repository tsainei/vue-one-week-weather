<template>
  <div class="results">
    <div class="card-1">
      <div class="cards">
        <div class="today">
          <h2>{{ current.name }}</h2>
          <div class="daily">
            <img
              :src="
                require(`../assets/weather_elements/${
                  weathers[current.weather[0].main]
                }.svg`)
              "
              width="220px"
              alt
            />
            <p>{{ parseInt(current.main.temp) }} °C</p>
          </div>
          <h2>{{ current.weather[0].main }}</h2>
        </div>
        <div class="todayDetails">
          <h3>Feels Like {{ parseInt(current.main.feels_like) }}° C</h3>
          <h3>
            <img width="64" src="../assets/weather_elements/humidity.png" alt />
            {{ current.main.humidity }}%
          </h3>
          <h3>
            <img src="../assets/weather_elements/down.png" alt />
            {{ parseInt(current.main.temp_min) }}°
          </h3>
          <h3>
            <img src="../assets/weather_elements/up.png" alt />
            {{ parseInt(current.main.temp_max) }}°
          </h3>
          <h3>
            <img src="../assets/weather_elements/wind.png" alt />
            {{ current.wind.speed }} km/h
          </h3>
        </div>
      </div>
    </div>
    <div class="card-2">
      <div class="daysDetail">
        <span
          style="
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-left: 10px;
          "
          v-for="data in daily.daily"
          :key="data"
        >
          <p>
            {{ data.day }}
          </p>
          <img
            :src="
              require(`../assets/weather_elements/${
                weathers[data.weather[0].main]
              }.svg`)
            "
            width="100px"
            alt
          />
          <p>{{ data.weather[0].main }}</p>
          <p>
            {{ parseInt(data.temp.day) }}° / {{ parseInt(data.temp.night) }}°
          </p>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Cards',
  props: ['daily', 'current'],
  data() {
    return {
      weathers: {
        Snow: 'snowy',
        Clouds: 'cloudy',
        Rain: 'rainy',
        Clear: 'sunny',
        Thunderstorm: 'thunder',
      },
      days: [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ],
      daysIndex: [],
    };
  },

  mounted() {
    var currentDate = new Date();
    var nextWeek = new Date(currentDate.getTime() + 7 * 24 * 60 * 60 * 1000);
    var days = [];
    while (currentDate <= nextWeek) {
      days.push(new Date(currentDate).getDay());
      currentDate.setDate(currentDate.getDate() + 1);
    }
    this.daysIndex = days.slice(1);
  },
  watch: {
    daily: function () {
      for (let i = 0; i < this.daysIndex.length; i++) {
        this.daily[i] = this.days[this.daysIndex[i]];
      }
    },
  },
};
</script>

<style>
.results {
  display: flex;
  flex-direction: column;
  width: 60vw !important;
}
.cards {
  display: flex;
  width: 100%;
  margin-top: 20px;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
.card-1 {
  width: 100%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}
.today {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.todayDetails {
  display: flex;
  margin-top: 30px;
  flex-direction: column;
  align-items: flex-start;
}
.daily {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  font-size: 90px;
}
.card-2 {
  width: 100%;
  height: 300px;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: stretch;
  overflow: auto;
}
.days {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  font-size: 23px;
}
.daysDetail {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
  font-size: 23px;
}
@media (max-width: 1300px) {
  .card-2 {
    align-items: baseline;
  }
  .results {
    width: 90vw !important;
  }
}
</style>
