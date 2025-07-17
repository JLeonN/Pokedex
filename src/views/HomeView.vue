<template>
  <header class="header">
    <label for="pokemonInput">
      Escribe el nombre o ID del Pokémon:
      <input type="text" id="pokemonInput" v-model="pokemonID" />
    </label>
    <button class="searchButton" @click="searchPokemon">Buscar pokemon!</button>
  </header>

  <main class="main" v-if="Object.entries(pokemonData).length > 0">
    <section class="pokemonCard">
      <div class="nameImage">
        <h1 class="pokemonName">{{ pokemonData.name }}</h1>
        <img :src="pokemonData.sprites.front_default" :alt="pokemonData.name" />
      </div>
      <ul class="type">
        <h2>Tipo:</h2>
        <li
          v-for="(type, key) in pokemonData.types"
          :key="key"
          :class="type.type.name"
        >
          <span>{{ type.type.name }}</span>
        </li>
      </ul>
      <ul class="stats">
        <h2>Estadísticas:</h2>
        <li v-for="(stat, key) in pokemonData.stats" :key="key">
          <span>{{ stat.stat.name }} -> {{ stat.base_stat }}</span>
        </li>
      </ul>
    </section>
  </main>
</template>

<script>
import axios from "axios";
import { pokeapi } from "@/api/pokeapi";

export default {
  name: "App",
  data() {
    return {
      pokemonData: {},
      pokemonID: "",
    };
  },

  methods: {
    async searchPokemon() {
      try {
        const respuesta = await axios.get(`${pokeapi}/${this.pokemonID}`);
        this.pokemonData = respuesta.data;
        console.log(respuesta.data);
        return respuesta.data;
      } catch (error) {
        alert("Pokemon was not found");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "/src/assets/lol.scss";
</style>
