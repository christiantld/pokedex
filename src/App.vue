<template>
  <div id="app">
    <div v-for="(poke, index) in pokemons" :key="index">
      <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
    </div>
  </div>
</template>

<script>
import http from "./services/axios";
import Pokemon from "./components/Pokemon.vue";
export default {
  name: "App",
  components: { Pokemon },
  data() {
    return {
      pokemons: [],
    };
  },

  methods: {},

  created() {
    http.get("pokemon?limit=151&offset=0").then((res) => {
      const { results } = res.data;
      this.pokemons = results;
      return results;
    });
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
}
</style>
