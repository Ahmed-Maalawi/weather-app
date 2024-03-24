<script setup>
import SearchInput from "@/components/searchInput.vue";
import {ref} from "vue";
import WeatherCard from "@/components/weatherCard.vue";


const places = ref([]);

const addPlace = (data) => {
  places.value.push(data);
}

const deletePlace = (name) => {
  if (confirm('Are you sure to to delete this card ?')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>

<template>
  <main>

  <!--    Date  -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-us', {
            weekday: 'long',
            year: 'numeric',
            month: 'long',
            day: 'numeric'
        })
      }}
    </div>

    <!--    search    -->
    <search-input @place-data="addPlace"></search-input>

    <!--  weather cards  -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, index) in places" :key="index">
        <weather-card :place="place" @delete-place="deletePlace"></weather-card>
      </div>
    </div>
  </main>
</template>
