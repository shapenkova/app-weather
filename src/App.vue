<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input type="text" class="weather-form__input" placeholder="Enter city" v-model="searchQuery" @keyup.enter="weatherSearch">
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>
      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">
        <div class="card" v-if="error">Error</div>
        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} °{{ unit }}</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>

    </div>

    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/bg.jpg" alt="App Background">
        <img class="weather-bg__img overcast" src="./assets/overcast.jpg" alt="Overcast">
        <img class="weather-bg__img partly-cloudy" src="./assets/partly-cloudy.jpg" alt="Partly Cloudy">
        <img class="weather-bg__img sunny" src="./assets/sunny.jpg" alt="Sunny">
        <img class="weather-bg__img light-snow" src="./assets/snow.jpg" alt="Light snow">
        <img class="weather-bg__img light-rain" src="./assets/rain.jpg" alt="Light rain">
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
    };
  },

  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny';
      } else if (this.description.includes('Overcast')) {
        return 'overcast';
      } else if (this.description.includes('Partly cloudy')) {
        return 'partly-cloudy';
      } else if (this.description.includes('Light snow')) {
        return 'light-snow';
      }  else if (this.description.includes('Light rain')) {
        return 'light-rain';
      } else {
        return '';
      }
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
          console.log(data);
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
