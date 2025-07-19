<template>
  <header class="header">
    <label for="pokemonInput">
      Escribe el nombre o ID del Pokémon:
      <input
        type="text"
        id="pokemonInput"
        v-model="pokemonID"
        @keyup.enter="buscarPokemon"
      />
    </label>
    <button class="searchButton" @click="buscarPokemon">Buscar Pokémon!</button>
  </header>
  <main class="main" v-if="Object.entries(datosPokemon).length > 0">
    <section class="pokemonCard">
      <div class="nameImage">
        <h1 class="pokemonName">
          #{{ datosPokemon.id }} - {{ datosPokemon.name }}
        </h1>
        <img
          :src="datosPokemon.sprites.front_default"
          :alt="datosPokemon.name"
        />
      </div>
      <ul class="type">
        <h2>Tipo:</h2>
        <li
          v-for="(tipo, clave) in datosPokemon.types"
          :key="clave"
          :class="tipo.type.name"
        >
          <span>{{ tipo.type.name }}</span>
        </li>
      </ul>
      <ul class="stats">
        <h2 class="statsH2">Estadísticas:</h2>
        <li v-for="(stat, clave) in datosPokemon.stats" :key="clave">
          <span>{{ stat.stat.name }} -> {{ stat.base_stat }}</span>
        </li>
      </ul>
    </section>
    <MasInformacion
      :evoluciones="cadenaEvolutiva"
      :id-pokemon="datosPokemon.id"
    />
  </main>
</template>

<script>
import axios from "axios";
import { pokeapi } from "@/api/pokeapi";
import MasInformacion from "@/components/MasInformacion.vue";

export default {
  name: "HomeView",
  components: {
    MasInformacion,
  },
  data() {
    return {
      datosPokemon: {},
      pokemonID: "",
      cadenaEvolutiva: [],
    };
  },
  methods: {
    async buscarPokemon() {
      try {
        const respuesta = await axios.get(`${pokeapi}/${this.pokemonID}`);
        this.datosPokemon = respuesta.data;

        const especie = await axios.get(respuesta.data.species.url);
        const cadena = await axios.get(especie.data.evolution_chain.url);

        this.cadenaEvolutiva = this.obtenerCadenaEvolutiva(cadena.data.chain);
      } catch (error) {
        alert("No se encontró el Pokémon");
        this.datosPokemon = {};
        this.cadenaEvolutiva = [];
      }
    },

    obtenerCadenaEvolutiva(cadena) {
      const evoluciones = [];
      let actual = cadena;

      while (actual) {
        evoluciones.push({
          nombre: actual.species.name,
          id: this.obtenerIDDesdeURL(actual.species.url),
        });
        actual = actual.evolves_to[0];
      }

      return evoluciones;
    },

    obtenerIDDesdeURL(url) {
      const partes = url.split("/");
      return partes[partes.length - 2];
    },
  },
};
</script>

<style lang="scss" scoped>
@import "/src/assets/lol.scss";
</style>
