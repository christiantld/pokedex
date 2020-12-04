<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img
            @click="mudarSprite"
            :src="isFront ? pokemon.front : pokemon.back"
            alt="Placeholder image"
            class="pokemon-img"
          />
        </figure>
      </div>
      <div class="card-content px-0 py-2">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">
              {{ pokemonNum | numFormated }} - {{ name | uppercase }}
            </p>
            <p class="subtitle is-6">{{ pokemon.type | uppercase }}</p>
          </div>
        </div>

        <div class="content"></div>
      </div>
    </div>
  </div>
</template>
<script>
import http from "../services/axios";
export default {
  props: {
    name: String,
    url: String,
  },
  data() {
    return {
      pokemon: {},
      isFront: true,
      notFound: false,
    };
  },
  methods: {
    mudarSprite() {
      this.isFront = !this.isFront;
    },
  },

  computed: {
    pokemonNum() {
      return this.url.split("/")[6];
    },
  },

  filters: {
    uppercase(value) {
      const newName = value[0]?.toUpperCase() + value.slice(1);
      return newName;
    },

    numFormated(value) {
      if (value.length === 2) {
        return `0${value}`;
      }
      if (value.length === 1) {
        return `00${value}`;
      }
      return value;
    },
  },

  created() {
    try {
      http.get(this.url).then((res) => {
        const { sprites, types } = res.data;

        const data = {
          type: types[0]?.type.name,
          front: sprites.front_default,
          back: sprites.back_default,
        };
        this.pokemon = data;
      });
    } catch (error) {
      this.notFound = true;
    }
  },
};
</script>
<style>
#pokemon {
  margin: 5px;
}
.pokemon-img {
  cursor: pointer;
}

.card {
  width: 250px;
}
</style>