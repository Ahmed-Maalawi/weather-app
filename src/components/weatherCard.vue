<script setup>

  import {ref} from "vue";
  import BorderLine from './BorderLine.vue';
  import WeatherForecastDay from "@/components/weatherForecastDay.vue";
  import WeatherInfo from "@/components/weatherInfo.vue";

  // eslint-disable-next-line no-undef
  defineProps({
    place: Object,
  });

  // eslint-disable-next-line no-undef
  const emit = defineEmits(['delete-place'])

  const showDetails = ref(false);

  const removePlace = (placeName) => {
    emit('delete-place', placeName)
    showDetails.value = false
  }


</script>

<template>
  <div
      v-if="place"
      class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden bg-blue-500"
      :class="1 === place.current.is_day? 'bg-day': 'bg-night'"
  >
    <!-- Location & time -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">{{ new Date(place.location.localtime).getHours() + ':' + new Date(place.location.localtime).getMinutes() }}</h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="text-9xl mb-2 -mr-4">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, index) in place.forecast.forecastday" :key="index">
      <weather-forecast-day :day="day"/>
    </div>

    <!-- info -->
    <Transition name="fade">
      <div v-show="showDetails">
        <weather-info
            :place="place"
            @close-info="showDetails = false"
            @remove-place="removePlace(place.location.name)"
        />
      </div>
    </Transition>


    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="showDetails = true" >More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i></button>
    </div>
  </div>
</template>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>