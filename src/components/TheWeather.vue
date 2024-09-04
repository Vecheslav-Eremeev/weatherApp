<template>
  <div class="container p-0">
    <div class="d-flex" v-if="success">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">Сегодня</h2>
          <p class="text-light date mb-0">{{ localDate }}</p>
          <small>{{ localTime }}</small>
          <h2 class="place">
            <i class="fa fa-location"
              >{{ name }} <small>{{ country }}</small></i
            >
          </h2>
          <div class="temp">
            <h1 class="weather-temp">{{ temperature }}&deg;</h1>
            <h2 class="text-light">
              {{ description }} <img :src="iconUrl" :alt="description" />
            </h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <h2 class="text-center mt-2 forecast">Прогноз на 4 дня</h2>
        <Forecast :cityName="city" />
        <div id="div_form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input
              type="buton"
              value="Сменить город"
              class="btn change_btn btn-primary"
              @click="changeLocatiob"
            />
          </form>
        </div>
      </div>
    </div>
    <div v-else-if="loading" class="text-center" style="color: white">
      <h4>Загрузка...</h4>
    </div>
    <div v-else class="text-center" style="color: white">
      <h4>По вашему запросу ничего не найдено</h4>
    </div>
  </div>
</template>

<script>
import Forecast from "./Forecast.vue";
import axios from "axios";
import moment from 'moment';
import 'moment/dist/locale/ru';

export default {
  components: {
    Forecast,
  },
  props: {
    city: String,
  },
  data() {
    return {
      success: false,
      loading: true,
      name: "",
      country: "",
      temperature: null,
      description: null,
      iconUrl: null,
      localDate: null,
      localTime: null
    };
  },
  async created() {
    this.loading = true;
    const api_key = "3de86b1cc29a7e2c02914bf19e0de2eb";
    const base_url = "https://api.openweathermap.org/data/2.5/";
    const weatherData = await axios
      .get(`${base_url}weather?q=${this.city}&units=metric&APPID=${api_key}`)
      .then((r) => {
        this.success = true;
        this.loading = false;
        return r.data;
      })
      .catch((error) => {
        console.error("Error featchig data", error);
        this.success = false;
        this.loading = false;
      });

    this.name = weatherData.name;
    this.country = weatherData.sys.country;
    this.temperature = Math.round(weatherData.main.temp);
    this.description = weatherData.weather[0].main;

    this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;

    const now = moment(new Date())
    moment.locale('ru')
    this.localDate = now.format('dddd DD MMMM YYYY')
    this.localTime = now.format('HH:mm:ss')
  },
  methods: {
    changeLocatiob() {
      window.location.reload();
    }
  }
};
</script>

<style scoped>
@import url("../assets/css/weather.css");
</style>
