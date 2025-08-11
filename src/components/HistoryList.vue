<script setup>
import HistoryItem from "./HistoryItem.vue";

// props
const { historyList } = defineProps(["historyList"]);
</script>

<template>
  <section v-if="historyList">
    <HistoryItem v-for="item in historyList" :historyItem="item" />
  </section>
</template>
