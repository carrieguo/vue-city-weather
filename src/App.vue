<template>
  <div id="app" class="container-fluid">
    <h1 class="text-center">China Map with Weather Info</h1>
    <div class="row">
      <div class="col-12">
        <Map @show-weather="setCityName" />
      </div>
    </div>
    <!-- Popover for displaying weather info -->
    <transition name="popover-fade">
      <div v-if="selectedCity" class="popover bs-popover-auto" role="tooltip" :style="popoverStyle">
        <div class="arrow"></div>
        <h3 class="popover-header">{{ selectedCity }} 一周天气</h3>
        <div class="popover-body">
          <WeatherInfo :city-name="selectedCity" />
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Map from './components/Map.vue';
import WeatherInfo from './components/WeatherInfo.vue';

const selectedCity = ref('');
const popoverPosition = ref({ top: 0, left: 0 });

const setCityName = (cityName, position) => {
  selectedCity.value = cityName;
  popoverPosition.value = position;
};

const popoverStyle = computed(() => {
  return {
    top: `${popoverPosition.value.top}px`,
    left: `${popoverPosition.value.left}px`,
  };
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
}

.popover {
  max-width: 400px;
}

@media (max-width: 576px) {
  .popover {
    max-width: 90%;
    left: 5% !important;
    right: 5% !important;
  }
}

.popover-fade-enter-active, .popover-fade-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}

.popover-fade-enter, .popover-fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
  transform: translateY(-10px);
}
</style>