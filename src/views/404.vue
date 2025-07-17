<template>
  <div class="contenedor404">
    <h1>Error 404</h1>
    <p>No se encontró el pokémon</p>
    <img src="/PikachuTriste.png" alt="Pikachu triste" class="imagen404" />
    <router-link to="/" class="botonInicio">Volver al inicio</router-link>
  </div>
</template>

<script>
export default {
  name: 'PaginaNoEncontrada'
}
</script>

<style scoped src="../assets/404.scss"></style>
