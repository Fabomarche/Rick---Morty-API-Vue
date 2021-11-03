<template>
  <div>
    <h2>Characters</h2>
    <div class="search">
      <input
        v-model="search"
        type="text"
        placeholder="Search"
        v-on:keyup.enter="searchData"
      />
      <button v-on:click="searchData" class="page-buttons">buscar</button>
    </div>

    <div class="container">
      <div
        class="characters-container"
        v-for="character of characters"
        v-bind:key="character.id"
        v-bind:character="character"
      >
        <div class="character-card" v-on:click="openDetail">
          <img v-bind:src="character.image" alt="image character.name" />
          <div class="character-title">
            <h3>{{ character.name }}</h3>
          </div>
        </div>
      </div>
    </div>
    <nav class="container-pagination" role="navigation" aria-label="pagination">
      <button v-on:click="changePage(page - 1)" class="page-buttons">-</button>
      <h6 class="number-page">{{ page }}</h6>
      <button v-on:click="changePage(page + 1)" class="page-buttons">+</button>
    </nav>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CharactersConteiner",
  componets: {},
  data: function () {
    return {
      search: "",
      characters: [],
      page: 1,
      pages: 1,
      currentCharacter: {},
    };
  },

  created() {
    this.fetch();
  },

  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search,
      };

      let url = "https://rickandmortyapi.com/api/character";
      axios
        .get(url, { params })
        .then((res) => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
          console.log(res.data.info);
        })
        .catch((err) => console.log(err));
    },

    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },

    searchData() {
      this.page = 1;
      this.fetch();
    },

    openDetail() {
      this.fetchCharacter(id);
    },
    async fetchCharacter(id) {
      let result = await axios.get(
        "https://rickandmortyapi.com/api/character/${id}/"
      );
      this.currentCharacter = result.data;
      console.log(this.currentCharacter);
    },
  },
};
</script>
