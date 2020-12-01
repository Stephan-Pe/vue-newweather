<template>
  <v-container v-bind:class="{ hot: backgroundCheck }">
    <v-row>
      <v-col>
        <v-text-field
          v-model="city"
          v-on:keyup.enter="fetchWeather"
        ></v-text-field>
        <br />
        <div v-if="forecast.main">
          {{ forecastTemperature }}
          <div class="weather__date">{{ dateApp() }}</div>
          <div class="wind__dir">{{ windDir() }}</div>
          <div class="wind__speed">{{ windSpeed() }}</div>
           <div class="weather__icon">
          <img
            v-bind:src="
              require('@/assets/' + forecast.weather[0].icon + '.png')
            "
          /></div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapFields } from "vuex-map-fields";

export default {
  name: "Search",
  components: {},
  data() {
    return {
      forecast: {},
    };
  },
  computed: {
    ...mapFields(["city"]),
    forecastTemperature() {
      return Math.round(this.forecast.main.temp) + " Grad Celcius";
    },
    backgroundCheck() {
      if (this.forecast.main && this.forecast.main.temp > 10) {
        return true;
      } else {
        return false;
      }
    },
  },
  methods: {
    fetchWeather() {
      this.$store.dispatch("fetchWeather", this.city).then((response) => {
        this.forecast = response;
      });
    },
    dateApp() {
      let d = new Date();
      let months = [
        "Januar",
        "Februar",
        "März",
        "April",
        "Mai",
        "Juni",
        "July",
        "August",
        "September",
        "Oktober",
        "November",
        "Dezember",
      ];
      let days = [
        "Sonntag",
        "Montag",
        "Dienstag",
        "Mittwoch",
        "Donnerstag",
        "Freitag",
        "Sonnabend",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    windSpeed() {
      let speed = this.forecast.wind.speed;
      return `Die Windgeschwindigkeit beträgt ${speed} Knoten!`;
    },
    windDir() {
      let degree = this.forecast.wind.deg;
      if (degree > 337.5) return "Wind aus Richtung Nord";
      if (degree > 292.5) return "Wind aus Richtung Nord West";
      if (degree > 247.5) return "Wind aus Richtung West";
      if (degree > 202.5) return "Wind aus Richtung Süd West";
      if (degree > 157.5) return "Wind aus Richtung Süd";
      if (degree > 122.5) return "Wind aus Richtung Süd Ost";
      if (degree > 67.5) return "Wind aus Richtung Ost";
      if (degree > 22.5) {
        return "Wind aus Richtung Nord Ost";
      }
      return "Kein Wind";
    },
  },
};
</script>
<style>
.container {
  height: 100%;
  color: aliceblue;
}
.v-input {
  color: aliceblue;
}
.home {
  background-attachment: fixed;
  background: url("~@/assets/snowboard.jpg"), radial-gradient(circle, rgba(193,215,235,.0.5) 35%, rgba(74,79,80,0.5) 100%);
  background-blend-mode: multiply;
  background-position: center;
  background-repeat: no-repeat;
  height: 100%;
}

.hot {
  background-attachment: fixed;
  background: url("~@/assets/beach.jpg"),
    radial-gradient(
      circle,
      rgba(5, 84, 153, 0.5) 35%,
      rgba(11, 165, 196, 0.5) 100%
    );
  background-blend-mode: multiply;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
