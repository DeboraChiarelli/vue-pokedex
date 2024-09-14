<template>
    <div v-if="pokemon">
    <h3>{{ pokemon.name | capitalize }}</h3>
    <img :src="pokemon.sprites.front_default" :alt="pokemon.name" />
    <p>Height: {{ pokemon.height }}</p>
    <p>Weight: {{ pokemon.weight }}</p>
    <p>Base experience: {{ pokemon.base_experience }}</p>
    <button @click="$emit('save-pokemon', pokemon)">Add to Pokedex</button>
  </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemon: Object,
    },
    filters: {
      capitalize(value) {
        if (!value) return '';
        value = value.toString();
        return value.charAt(0).toUpperCase() + value.slice(1);
      },
    },
  };
  </script>