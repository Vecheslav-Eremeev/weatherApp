<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">{{ title }}</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input
            type="text"
            class="input form-control"
            v-model="city"
            placeholder="Введите название города.."
          />
        </div>
        <button class="btn btn-search btn-primary" @click="searchWeather">
          Поиск <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
    <br>
    <TheWeather :city="city" v-if="showWeather"/>
  </div>
</template>

<script>
import TheWeather from './components/TheWeather.vue';

export default {
  name: "App",
  components: {
    TheWeather,
  },
  data() {
    return {
      city: "",
      showWeather: false,
      title: 'Погодное приложение'
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    },
  },
};
</script>

<style>
@import url('./assets/css/main.css');
</style>
