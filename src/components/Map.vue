<template>
  <div id="mapContainer" style="width: 100%; height: 80vh"></div>
</template>

<script setup>
import { onMounted, ref } from "vue";

const map = ref(null);
const cities = [
  { "name": "北京", "lat": 39.9042, "lng": 116.4074 },
  { "name": "天津", "lat": 39.0842, "lng": 117.2008 },
  { "name": "上海", "lat": 31.2304, "lng": 121.4737 },
  { "name": "重庆", "lat": 29.563, "lng": 106.5516 },
  { "name": "哈尔滨", "lat": 45.8038, "lng": 126.534 },
  { "name": "长春", "lat": 43.8171, "lng": 125.3235 },
  { "name": "沈阳", "lat": 41.8057, "lng": 123.4315 },
  { "name": "呼和浩特", "lat": 40.8427, "lng": 111.748 },
  { "name": "石家庄", "lat": 38.0428, "lng": 114.5149 },
  { "name": "太原", "lat": 37.8706, "lng": 112.5489 },
  { "name": "西安", "lat": 34.3416, "lng": 108.9398 },
  { "name": "郑州", "lat": 34.7466, "lng": 113.6254 },
  { "name": "济南", "lat": 36.6741, "lng": 117.0009 },
  { "name": "南京", "lat": 32.0572, "lng": 118.8062 },
  { "name": "合肥", "lat": 31.8612, "lng": 117.2827 },
  { "name": "武汉", "lat": 30.5928, "lng": 114.3055 },
  { "name": "长沙", "lat": 28.2282, "lng": 112.9388 },
  { "name": "南昌", "lat": 28.682, "lng": 115.8579 },
  { "name": "广州", "lat": 23.1291, "lng": 113.2644 },
  { "name": "南宁", "lat": 22.817, "lng": 108.3665 },
  { "name": "海口", "lat": 20.0204, "lng": 110.3542 },
  { "name": "成都", "lat": 30.5728, "lng": 104.0668 },
  { "name": "贵阳", "lat": 26.6466, "lng": 106.6302 },
  { "name": "昆明", "lat": 24.8801, "lng": 102.8329 },
  { "name": "拉萨", "lat": 29.6501, "lng": 91.1194 },
  { "name": "西宁", "lat": 36.6232, "lng": 101.7782 },
  { "name": "银川", "lat": 38.4872, "lng": 106.2309 },
  { "name": "乌鲁木齐", "lat": 43.8256, "lng": 87.6169 },
  { "name": "台北", "lat": 25.033, "lng": 121.5654 },
  { "name": "香港", "lat": 22.3193, "lng": 114.1694 },
  { "name": "澳门", "lat": 22.1987, "lng": 113.5439 }
];

const emit = defineEmits(["show-weather"]);

const showWeatherInfo = (cityName, position) => {
  emit("show-weather", cityName, position);
};

const initMap = () => {
  map.value = new AMap.Map("mapContainer", {
    zoom: 4,
    center: [104, 35],
  });

  cities.forEach((city) => {
    const marker = new AMap.Marker({
      position: new AMap.LngLat(city.lng, city.lat),
      title: city.name,
    });

    marker.setMap(map.value);

    marker.on("click", (e) => {
      const position = { top: e.pixel.y, left: e.pixel.x };
      showWeatherInfo(city.name, position);
    });
  });
};

onMounted(() => {
  initMap();
});
</script>

<style scoped>

</style>
