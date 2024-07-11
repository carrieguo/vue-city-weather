<template>
  <div>
    <transition name="fade">
      <div v-if="!loading && weatherData" class="weather-info">
        <ul class="list-unstyled">
          <li v-for="(day, index) in weatherData.daily" :key="index">
            {{ day.fxDate }}: 白天 {{ day.textDay }}，夜间{{ day.textNight }}，最高温度{{ day.tempMax }}℃，最低温度{{ day.tempMin }}℃
          </li>
        </ul>
      </div>
      <div v-else-if="loading || !weatherData" class="loading-text">
        {{ loading ? '加载中...' : '未能获取天气信息，请稍后再试。' }}
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import { defineProps } from 'vue';

const props = defineProps({
  cityName: String,
});

const weatherData = ref(null);
const loading = ref(false);

const fetchWeatherData = async (cityName) => {
  if (!cityName) return;
  loading.value = true;
  try {
    const locationResponse = await axios.get('https://geoapi.qweather.com/v2/city/lookup', {
      params: {
        location: cityName,
        key: 'b80a5011698f4b2da0e1c209336632f8',
      },
    });
    const locationId = locationResponse.data.location[0].id;

    const weatherResponse = await axios.get('https://devapi.qweather.com/v7/weather/7d', {
      params: {
        location: locationId,
        key: 'b80a5011698f4b2da0e1c209336632f8',
      },
    });

    weatherData.value = weatherResponse.data;
  } catch (error) {
    console.error('Error fetching weather data:', error);
    weatherData.value = null;
  } finally {
    loading.value = false;
  }
};

fetchWeatherData(props.cityName);

watch(() => props.cityName, (newCityName) => {
  fetchWeatherData(newCityName);
});
</script>

<style scoped>
.weather-info, .loading-text {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 10px;
  text-align: center;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
</style>