<template>
  <div id="app">
    <h1>My Pokedex</h1>
    <SearchPokemon />
  </div>
</template>

<script>
import SearchPokemon from './components/SearchPokemon.vue';

export default {
  components: {
    SearchPokemon,
  },
};
</script>
