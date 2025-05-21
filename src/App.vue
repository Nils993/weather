<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input
          v-model="searchQuery"
          type="text"
          class="weather-form__input"
          placeholder=" Enter city"
        />
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>
      <div class="card weather-load" v-if="loading">Loading...</div>
      <div
        class="weather-info"
        v-show="!error && location && temperature !== 0 && description"
      >
        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg">
      <div>
        <img
          class="weather-bg__img bg"
          src="./assets/img/bg.jpg"
          alt=" App background"
        />
        <img
          class="weather-bg__img overcast"
          src="./assets/img/overcast.jpg"
          alt="Overcast"
        />
        <img
          class="weather-bg__img partly-cloudy"
          src="./assets/img/cloudly.jpg"
          alt="Partly"
        />
        <img
          class="weather-bg__img clear"
          src="./assets/img/lear.jpg"
          alt="Clear"
        />
        <img
          class="weather-bg__img sunny"
          src="./assets/img/sunny.jpg"
          alt="Sunny"
        />
        <img
          class="weather-bg__img light-rain"
          src="./assets/img/Lightrain.jpg"
          alt="Light rain"
        />
        <img
          class="weather-bg__img rain"
          src="./assets/img/rain.jpg"
          alt="rain"
        />
        <img
          class="weather-bg__img patchy-thunder"
          src="./assets/img/Patchythunder.jpg"
          alt="Patchy thunder"
        />
        <img
          class="weather-bg__img mist"
          src="./assets/img/Mist.jpg"
          alt="Mist"
        />
        <img
          class="weather-bg__img snow"
          src="./assets/img/snow.jpg"
          alt="snow"
        />
        <img
          class="weather-bg__img blizzard"
          src="./assets/img/Blizzard.jpg"
          alt="Blizzard"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: "",
      temperature: 0,
      description: "",
      loading: false,
      error: false,
      searchQuery: "",
    };
  },
  computed: {
    weatherClass() {
      const conditionsMap = {
        Sunny: "sunny",
        Clear: "clear",
        Overcast: "overcast",
        Cloudy: "partly-cloudy",
        cloudy: "partly-cloudy",
        Mist: "mist",
        Fog: "mist",
        thunder: "patchy-thunder",
        "Patchy rain nearby": "light-rain",
        "Light rain": "light-rain",
        rain: "rain",
        Snow: "snow",
        Blizzard: "blizzard",
      };
      for (const key in conditionsMap) {
        if (this.description.includes(key)) {
          return conditionsMap[key];
        }
      }
      return "";
    },
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `http://api.weatherapi.com/v1/current.json?key=3f7f045737ba4661827103617252005&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.searchQuery = "";
    },
  },
};
</script>
