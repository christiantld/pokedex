<template>
  <div id="app">
    <img src="./assets/pokemoncolor.png" width="260px" />
    <div class="search-div my-4">
      <input
        v-if="!notFound"
        type="text"
        name=""
        id=""
        class="input is-rounded"
        placeholder="Buscar Pokemon"
        v-model.trim="pokemon"
        @keypress.enter="search"
        @keypress.backspace="clean"
      />
      <button
        v-if="!notFound"
        class="button is-rounded is-warning ml-2"
        @click="search"
      >
        Buscar
      </button>
    </div>
    <p>{{ pokemonNumber }}</p>
    <div class="poke-list">
      <Loading v-if="loading" />
      <div v-if="notFound">
        <img
          src="./assets/oficerJenny.png"
          width="350px"
          alt=""
          class="mt-5 mb-2"
        />
        <p class="is-size-3">Desculpe, não encontramos o seu Pokémon</p>
      </div>
      <div v-else v-for="poke in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" />
      </div>
    </div>
    <button
      v-if="!loading"
      class="button is-rounded is-warning ml-2 my-4"
      @click="clean"
    >
      Listar Todos
    </button>
  </div>
</template>

<script>
import http from "./services/axios";
import Pokemon from "./components/Pokemon.vue";
import Loading from "./components/Loading.vue";
export default {
  name: "App",
  components: { Pokemon, Loading },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      pokemon: "",
      loading: false,
      filtered: true,
      notFound: false,
    };
  },

  methods: {
    search() {
      this.filteredPokemons = this.pokemons;
      if (this.pokemon) {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name === this.pokemon.toLowerCase()
        );
        if (this.filteredPokemons.length === 0) {
          console.log(this.pokemons);
          this.filteredPokemons = this.pokemons;
          this.notFound = true;
        }
        this.pokemon = "";
      }
    },

    clean() {
      this.notFound = false;
      this.filteredPokemons = this.pokemons;
      this.pokemon = "";
    },
  },

  computed: {
    // searchPokemon() {
    //   if (!this.pokemon) {
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter(
    //       (pokemon) => pokemon.name === this.pokemon.toLowerCase()
    //     );
    //   }
    // },
  },

  mounted() {
    this.loading = true;
    setTimeout(() => {
      http
        .get("pokemon?limit=151&offset=0")
        .then((res) => {
          const { results } = res.data;
          this.pokemons = results;
          this.filteredPokemons = results;
          return results;
        })
        .then(() => {
          this.loading = false;
        });
    }, 2000);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.search-div {
  display: flex;
  flex-direction: row;
  max-width: 450px;
  align-items: center;
}

.poke-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
