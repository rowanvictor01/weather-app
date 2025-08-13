<script setup>
import HistoryItem from "./HistoryItem.vue";

// props
const { historyList } = defineProps(["historyList"]);
</script>

<template>
  <section
    v-if="historyList"
    class="flex flex-col justify-center items-center h-[90vh] gap-8"
  >
    <HistoryItem v-for="item in historyList" :historyItem="item" />
  </section>
</template>
