<template>
  <div class="weather" :class="weatherClass">
    <div class="weather__container">
      <div class="card weather__form form">
        <input type="text" class="form__input" placeholder="Enter city" v-model="searchQuery" @keyup.enter="weatherSearch">
        <button class="form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather__load" v-if="loading">Loading...</div>
      <div class="weather__info" v-show="!error && location && temperature !== 0 && description">
        <div class="card" v-if="error">Error</div>
        <div class="weather__info-text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} °{{ unit }}</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather__bg">
      <div>
        <img class="weather__bg-img bg" src="./assets/bg.jpg" alt="App Background">
        <img class="weather__bg-img overcast" src="./assets/overcast.jpg" alt="Overcast">
        <img class="weather__bg-img partly-cloudy" src="./assets/partly-cloudy.jpg" alt="Partly Cloudy">
        <img class="weather__bg-img sunny" src="./assets/sunny.jpg" alt="Sunny">
        <img class="weather__bg-img light-snow" src="./assets/snow.jpg" alt="Light snow">
        <img class="weather__bg-img light-rain" src="./assets/rain.jpg" alt="Light rain">
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      searchQuery: '',
      loading: false,
      error: false,
      unit: 'C',
      weatherTypes: {
        'Sunny': 'sunny',
        'Overcast': 'overcast',
        'Partly cloudy': 'partly-cloudy',
        'Light snow': 'light-snow',
        'Light rain': 'light-rain',
      },
    };
  },

  computed: {
    weatherClass() {
      for (const [key, value] of Object.entries(this.weatherTypes)) {
        if (this.description.includes(key)) {
          return value;
        }
      }
      return '';
    }
  },

  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`https://api.weatherapi.com/v1/current.json?key=67918f1c403b42c686a102557242810&q=${this.searchQuery}`)
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
      })
      .catch(error => {
        this.loading = false;
        this.error = true;
        console.error(error);
      });
    },
    resetSearchQuery() {
      this.searchQuery = '';
    }
  }
}
</script>
