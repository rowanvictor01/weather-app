<script setup>
const { historyItem } = defineProps(["historyItem"]);
</script>

<template>
  <section>
    <span>City</span>
    <span>Temp</span>
    <span>Weather Desc</span>
    <button class="border">Search</button>
    <button class="border">Delete</button>
  </section>
</template>
