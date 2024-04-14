<script setup lang="ts">
import Table from "./components/table/index.vue";
</script>

<template>
  <div class="app">
    <Table class="app__table" />
  </div>
</template>

<style>
.app {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
