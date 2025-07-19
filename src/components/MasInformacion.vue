<template>
  <section class="masInformacion" v-if="evoluciones.length > 0">
    <h2 class="tituloMasInformacion">Cadena evolutiva</h2>
    <div class="contenedorEvoluciones">
      <template v-for="(pokemon, index) in evoluciones" :key="pokemon.id">
        <div class="pokemonEvolucion">
          <img
            :src="obtenerImagen(pokemon.id)"
            :alt="pokemon.nombre"
          />
          <span>{{ pokemon.nombre }}</span>
        </div>
        <span class="flecha" v-if="index !== evoluciones.length - 1">→</span>
      </template>
    </div>
  </section>
</template>

<script>
export default {
  name: "MasInformacion",
  props: {
    evoluciones: {
      type: Array,
      required: true,
    },
  },
  methods: {
    obtenerImagen(id) {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "/src/assets/lol.scss";
</style>