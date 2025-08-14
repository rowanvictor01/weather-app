<script setup>
import { ref } from "vue";

// emits
const emit = defineEmits(["input-city"]);

// data
const cityName = ref("");

// methods
function onCitySearch() {
  emit("input-city", cityName.value);

  cityName.value = "";
}
</script>

<template>
  <section>
    <form
      @submit.prevent="onCitySearch"
      class="relative border rounded-2xl w-[clamp(296px,70%,512px)] mx-auto mt-6 text-center"
    >
      <button
        type="submit"
        class="absolute left-2 top-2.5 w-[clamp(15px,30%,25px)] h-[clamp(15px,30%,25px)]"
      >
        <img src="../assets/icons/home.svg" alt="home icon" />
      </button>

      <input
        v-model="cityName"
        type="text"
        placeholder="Search"
        class="my-0 mx-auto w-[clamp(236px,30%,472px)] py-3 text-center"
      />

      <button
        type="submit"
        class="absolute right-2 top-2.5 w-[clamp(15px,30%,25px)] h-[clamp(15px,30%,25px)]"
      >
        <img src="../assets/icons/search.svg" alt="search icon" />
      </button>
    </form>
  </section>
</template>
