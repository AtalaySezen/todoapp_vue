<script setup>
import { RouterView } from "vue-router";
import Header from "./components/Header.vue";
</script>

<template>
  <Header />
  <RouterView />
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  max-width: 1100px;
  margin: 0 auto;
}
</style>
