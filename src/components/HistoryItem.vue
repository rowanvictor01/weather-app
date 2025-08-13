<script setup>
const { historyItem } = defineProps(["historyItem"]);
</script>

<template>
  <section
    class="border rounded-xl text-xl 2xl:text-2xl w-[clamp(290px,70%,1200px)] flex justify-between items-center"
  >
    <span class="">City</span>
    <span class="mr-6 md:mr-0">Temp</span>
    <span class="hidden md:inline mr-24">Weather Desc</span>

    <button class="border rounded-lg text-lg 2xl:text-xl py-1 xl:py-2 2xl:py-4">
      Check
    </button>
    <button class="border rounded-lg text-lg 2xl:text-xl py-1 xl:py-2 2xl:py-4">
      Delete
    </button>
  </section>
</template>
