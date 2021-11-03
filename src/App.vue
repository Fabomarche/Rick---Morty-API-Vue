<template>
  <div id="app">
    <Header />
    <CharactersConteiner />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import CharactersConteiner from "./components/containers/CharactersConteiner.vue";

export default {
  name: "App",
  components: {
    Header,
    CharactersConteiner,
  },
};
</script>

<style scoped>
@import "./assets/styles/style.css";
</style>
