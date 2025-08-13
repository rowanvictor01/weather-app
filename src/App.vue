<script setup>
import { ref, reactive, computed } from "vue";
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

let currentWeather = reactive({}); // for the parsed .json object with .data as property
const forecastList = reactive([]);

const currentState = reactive({
  isLoading: false,
  errorMsg: null,
});

const forecastState = reactive({
  isLoading: false,
  errorMsg: null,
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
      currentWeather = {
        name: data.name,
        country: data.sys.country,
        temperature: data.main.temp,
        icon: data.weather[0].icon,
        description: data.weather[0].description,
        humidity: data.main.humidity,
        windSpeed: data.wind.speed,
        feelsLike: data.main["feels_like"],
      };

      historyItem.current = currentWeather;
    })
    .catch((err) => console.error(err))
    .finally(() => (currentState.isLoading = false));

  forecastResponse
    .then((res) => res.json())
    .then((data) => {
      extractDays(data);

      historyItem.forecast = forecastList;
    })
    .catch((err) => console.error(`An error has occured: ${err}`))
    .finally(() => (forecastState.isLoading = false));

  searchHistory.push(historyItem.current);

  console.log(`5-day forecast: ${forecastList}`);
  console.log(`Search History: ${searchHistory}`);

  /*
    Have written the code for fetching data, parsing unix date to day of the week,
    and extracted five days from the list of days from the fetched data.
    Using console.log to check the contents of the arrays but not printing expected values.

    Check extractDays() function and check if it's working.
    Also check searchHistory's contents and how items are being pushed.
    
    Items might not be getting pushed correctly.
    
    Check individual block sections.
  */
}

function extractDays(data) {
  for (let i = 0; i < 5; i++) {
    const forecastReport = {
      day: convertDate(data.list[i]["dt_txt"]),
      condition: data.list[i].weather.icon,
      temperature: data.list[i].main["feels_like"],
    };

    forecastList.push(forecastReport);
  }
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
