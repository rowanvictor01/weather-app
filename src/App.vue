<script setup>
import { ref, reactive, watch, computed } from "vue";
import SearchBar from "./components/SearchBar.vue";
import HistoryList from "./components/HistoryList.vue";
import WeatherResults from "./components/WeatherResults.vue";

// data
const currentView = ref("home"); // "home" or "results"

const searchHistory = reactive([]); // array of objects
const historyItem = reactive({
  current: null,
  forecast: null,
});

const currentState = reactive({
  isLoading: false,
  errorMsg: null,
});

const forecastState = reactive({
  isLoading: false,
  errorMsg: null,
});

// computed properties
const isLoadingDone = computed(() => {
  return !currentState.isLoading && !forecastState.isLoading ? true : false;
});

// methods
function getWeatherData(city) {
  const sanitizedCity = city.toLowerCase().trim();

  const currentURL = `${
    import.meta.env.VITE_API_BASE_URL
  }/weather?q=${sanitizedCity}&appid=${
    import.meta.env.VITE_API_KEY
  }&units=metric`;

  const forecastURL = `${
    import.meta.env.VITE_API_BASE_URL
  }/forecast?q=${sanitizedCity}&appid=${
    import.meta.env.VITE_API_KEY
  }&units=metric`;

  currentState.isLoading = true;
  forecastState.isLoading = true;

  const currentResponse = fetch(currentURL);
  const forecastResponse = fetch(forecastURL);

  currentResponse
    .then((res) => res.json())
    .then((data) => {
      const currentDayWeather = {
        name: data.name,
        country: data.sys.country,
        temperature: data.main.temp,
        icon: data.weather[0].icon,
        description: data.weather[0].description,
        humidity: data.main.humidity,
        windSpeed: data.wind.speed,
        feelsLike: data.main["feels_like"],
      };

      historyItem.current = currentDayWeather;
    })
    .catch((err) => {
      currentState.errorMsg = err;
      console.log(
        "An error has occured on retrieving today's weather:",
        currentState.errorMsg
      );
    })
    .finally(() => (currentState.isLoading = false));

  forecastResponse
    .then((res) => res.json())
    .then((data) => {
      historyItem.forecast = extractDays(data);
    })
    .catch((err) => {
      forecastState.errorMsg = err;
      console.log(
        "An error has occured on retrieving this week's forecast:",
        forecastState.errorMsg
      );
    })
    .finally(() => (forecastState.isLoading = false));
}

function extractDays(data) {
  const fiveDayForecast = [];

  for (let i = 0; i < 5; i++) {
    const forecastReport = {
      day: convertDate(data.list[i]["dt_txt"]),
      conditionIcon: data.list[i].weather[i],
      temperature: data.list[i].main["feels_like"],
    };

    fiveDayForecast.push(forecastReport);
  }

  return fiveDayForecast;
}

function convertDate(dtTxt) {
  const date = new Date(dtTxt);

  const day = date.getDay();

  switch (day) {
    case 0:
      return "Sunday";
    case 1:
      return "Monday";
    case 2:
      return "Tuesday";
    case 3:
      return "Wednesday";
    case 4:
      return "Thursday";
    case 5:
      return "Friday";
  }
}

// watchers
watch(historyItem, () => {
  if (historyItem.current && historyItem.forecast) {
    const copy = {
      current: historyItem.current,
      forecast: historyItem.forecast,
    };

    searchHistory.push(copy);

    for (let i in historyItem) {
      historyItem[i] = null;
    }
  }
});

watch(isLoadingDone, () => {
  if (isLoadingDone) {
    currentView.value = "results";
  }
});
</script>

<template>
  <section>
    <SearchBar @input-city="getWeatherData" />
  </section>

  <section v-if="currentView === 'home'">
    <HistoryList :historyList="searchHistory" />
  </section>

  <section v-else-if="currentView === 'results'">
    <WeatherResults />
  </section>
</template>
