<script setup>
  import {reactive} from "vue";

  // eslint-disable-next-line no-undef
  const emit = defineEmits(['place-data'])
  const searchTerm = reactive({
    query: '',
    timeout: null,
    results: null
  });

  const handleSearch = () => {
    clearTimeout(searchTerm.timeout);
    searchTerm.timeout = setTimeout(async () => {
      if (searchTerm.query !== '') {
        const res = await fetch(
            `http://api.weatherapi.com/v1/search.json?key=e3904bb728014b34ba4203950242303&q=${searchTerm.query}&aqi=yes`
        )

        const data = await res.json();
        searchTerm.results = data;

      } else {
        searchTerm.results = null
      }

    }, 500)
  }

  const getWeather = async (id) => {
    const res = await fetch(
        `http://api.weatherapi.com/v1/forecast.json?key=e3904bb728014b34ba4203950242303&days=3&q=id:${id}&aqi=yes`
    )

    const data = await res.json();
    console.log(data)
    emit('place-data', data);

    searchTerm.query = '';
    searchTerm.results = null;


  }
</script>

<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
            v-model="searchTerm.query"
            type="text"
            placeholder="Search for a place"
            class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
            @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div
          v-for="place in searchTerm.results"
          :key="place.id"
          @click="getWeather(place.id)"
      >
        <button class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
          {{ place.name }} {{ place.region }} {{ place.country }}
        </button>
      </div>
    </div>
  </div>
</template>