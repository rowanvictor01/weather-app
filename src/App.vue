<script setup>
import { ref, reactive, computed } from "vue";
import SearchBar from "./components/SearchBar.vue";
import HistoryList from "./components/HistoryList.vue";
import WeatherResults from "./components/WeatherResults.vue";

// data
const currentView = ref("home"); // "home" or "results"
const searchHistory = reactive(["hi", "hello", "how re you", "asdf"]); // array of objects
let currentWeather = reactive({}); // for the parsed .json object with .data as property
const loading = ref();
const error = ref();

// methods
</script>

<template>
  <section>
    <SearchBar />
  </section>

  <section v-if="currentView === 'home'">
    <HistoryList :historyList="searchHistory" />
  </section>

  <section v-else-if="currentView === 'results'">
    <WeatherResults />
  </section>
</template>
