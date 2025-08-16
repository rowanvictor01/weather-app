<script setup>
const { results } = defineProps(["results"]);

// methods
function convertWindSpeed(windSpeed) {
  return Math.round(windSpeed * 3.6);
}

function buildIconURL(iconCode) {
  return `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
}
</script>

<template>
  <article class="mt-10 flex flex-col gap-6">
    <section class="border flex flex-col gap-2">
      <output>{{
        `${results.current.name}, ${results.current.country}`
      }}</output>
      <output>{{ results.current.temperature }}&deg;C</output>
      <output>{{ results.current.description }}</output>
    </section>

    <section class="flex flex-col gap-4">
      <div class="border flex flex-col gap-2">
        <figure>
          <img
            src="../assets/icons/drop.png"
            alt="humidity icon"
            class="w-8 h-8"
          />
        </figure>
        <output>{{ results.current.humidity }}%</output>
        <output>Humidity</output>
      </div>

      <div class="border flex flex-col gap-2">
        <figure>
          <img
            src="../assets/icons/wind.png"
            alt="humidity icon"
            class="w-8 h-8"
          />
        </figure>
        <output>{{ convertWindSpeed(results.current.windSpeed) }}Km/H</output>
        <output>Wind Speed</output>
      </div>

      <div class="border flex flex-col gap-2">
        <figure>
          <img
            src="../assets/icons/cold.png"
            alt="humidity icon"
            class="w-8 h-8"
          />
        </figure>
        <output>{{ results.current.feelsLike }}&deg;C </output>
        <output>Feels Like</output>
      </div>
    </section>

    <output>Upcoming Days</output>

    <section class="flex flex-col gap-4">
      <div v-for="day in results.forecast" class="border flex flex-col gap-2">
        <output>{{ day.day }}</output>
        <figure class="w-16 h-16"><img :src="buildIconURL(day.conditionIcon)"></img></figure>
        <output>{{ day.temperature }}%</output>
      </div>
    </section>
  </article>
</template>
