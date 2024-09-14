<template>
  <div>
    <h2>Search Pokemon</h2>
    <input v-model="pokemonName" type="text" placeholder="Enter Pokemon name" />
    <button @click="searchPokemon">Search</button>

    <PokemonCharacteristics v-if="pokemonData" :pokemon="pokemonData" @save-pokemon="savePokemon" />

    <h2>My Pokedex</h2>
    <ul>
      <li v-for="pokemon in pokedex" :key="pokemon.name">
        {{ pokemon.name | capitalize }}
      </li>
    </ul>
  </div>
</template>

<script>
import PokemonCharacteristics from './PokemonCharacteristics.vue';

export default {
  components: {
    PokemonCharacteristics,
  },
  data() {
    return {
      pokemonName: '',
      pokemonData: null,
      pokedex: [], // Lista dei Pokemon salvati
    };
  },
  methods: {
    async searchPokemon() {
      if (this.pokemonName) {
        try {
          const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonName.toLowerCase()}`);
          const data = await response.json();
          this.pokemonData = data;
        } catch (error) {
          console.error('Pokemon not found!', error);
          this.pokemonData = null;
        }
      }
    },
    savePokemon(pokemon) {
      // Salva il Pokemon nella lista
      this.pokedex.push(pokemon);
    },
  },
};
</script>