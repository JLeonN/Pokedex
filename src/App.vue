<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HomeView from './views/HomeView.vue';
</script>

<template>
  <header>

    <div class="wrapper">

      <nav>
        <HomeView />
        <RouterLink to="/"></RouterLink>
        <RouterLink to="/*"></RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>

</style>
