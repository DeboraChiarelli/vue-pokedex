<template>
    <div v-if="pokemon">
      <h3>{{ pokemon.name | capitalize }}</h3>
      <img :src="pokemon.sprites.front_default" :alt="pokemon.name" />
      <p>Height: {{ pokemon.height }}</p>
      <p>Weight: {{ pokemon.weight }}</p>
      <p>Base experience: {{ pokemon.base_experience }}</p>
  
      <!-- Mostra le statistiche -->
      <h4>Stats:</h4>
      <div v-for="stat in pokemon.stats" :key="stat.stat.name" class="stat">
        <span>{{ stat.stat.name | capitalize }}:</span>
        <progress :value="stat.base_stat" max="100">{{ stat.base_stat }}</progress>
      </div>
  
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
        return value.charAt(0).toUpperCase() + value.slice(1);
      },
    },
  };
  </script>
  
  <style scoped>
  /* Stile per il componente delle caratteristiche del Pokémon */
  h3 {
    color: #3b4cca; /* Colore del titolo */
  }
  
  img {
    width: 120px;
    margin-bottom: 10px;
  }
  
  .stat {
    margin: 10px 0;
  }
  
  /* Progress bar per le statistiche */
  progress {
    width: 100px;
    height: 20px;
    border-radius: 5px;
    -webkit-appearance: none;
    appearance: none;
  }
  
  progress::-webkit-progress-bar {
    background-color: #ccc;
    border-radius: 5px;
  }
  
  progress::-webkit-progress-value {
    background-color: #3b4cca;
    border-radius: 5px;
  }
  
  button {
    background-color: #ff0000;
    color: #ffffff;
    border: none;
    padding: 10px;
    border-radius: 5px;
    font-size: 14px;
    cursor: pointer;
    margin-top: 10px;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #cc0000;
  }
  </style>