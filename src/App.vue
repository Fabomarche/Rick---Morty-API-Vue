<template>
  <div id="app">
    <Header />
    <CharactersConteiner />
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import CharactersConteiner from "./components/containers/CharactersConteiner.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Header,
    CharactersConteiner,
    Footer,
  },
};
</script>

<style>
@import "./assets/styles/style.css";
</style>
