<template>
  <div class="bg-dark">
    <h2 class="text-center">Characters</h2>
    <b-container class="container d-flex justify-content-center">
      <b-row>
        <b-col>
          <b-form-input
            v-model="search"
            type="text"
            placeholder="Search"
            v-on:keyup.enter="searchData"
            class="w-100"
          />
        </b-col>

        <b-col>
          <b-button v-on:click="searchData" variant="dark cardButton"
            >Search</b-button
          >
        </b-col>
      </b-row>
    </b-container>

    <div class="container d-flex flex-wrap justify-content-center">
      <div
        class="mx-2 mt-3 mb-0"
        v-for="character of characters"
        v-bind:key="character.id"
        v-bind:character="character"
      >
        <b-card
          style="max-width: 20rem"
          class="text-center"
          bg-variant="dark"
          text-variant="white"
        >
          <b-card-img v-bind:src="character.image" alt="image character.name" />
          <b-card-title class="fs-2 mb-3">{{ character.name }}</b-card-title>
          <b-button
            href="#"
            class="cardButton"
            variant="dark"
            v-on:click="openDetail(character.id)"
            v-b-modal.modal-1
            >Details</b-button
          >
        </b-card>
      </div>
    </div>

    <nav class="container-pagination" role="navigation" aria-label="pagination">
      <b-button
        v-on:click="changePage(page - 1)"
        variant="dark"
        class="cardButton"
        >before</b-button
      >
      <h6 class="number-page">{{ page }}</h6>
      <b-button
        v-on:click="changePage(page + 1)"
        variant="dark"
        class="cardButton"
        >next</b-button
      >
    </nav>

    <div>
      <b-modal id="modal-1" title="Character Details">
        <b-modal-content class="d-flex flex-column align-items-center">
          <h3 class="fs-1">{{ currentCharacter.name }}</h3>
          <ul class="fs-4">
            <li>Specie: {{ currentCharacter.species }}</li>
            <li>Status: {{ currentCharacter.status }}</li>
            <li>Gender: {{ currentCharacter.gender }}</li>
          </ul>
          <img v-bind:src="currentCharacter.image" alt="image character.name" />
        </b-modal-content>
      </b-modal>
    </div>
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

    openDetail(id) {
      this.fetchCharacter(id);
    },
    async fetchCharacter(id) {
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );
      this.currentCharacter = result.data;
      console.log(this.currentCharacter);
    },
  },
};
</script>
