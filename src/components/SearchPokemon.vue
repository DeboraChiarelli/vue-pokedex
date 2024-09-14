<template>
  <div>
    <h2>Search Pokemon</h2>
    <input v-model="pokemonName" type="text" placeholder="Enter Pokemon name" />
    <button @click="searchPokemon">Search</button>

    <!-- Caratteristiche del Pokemon cercato -->
    <PokemonCharacteristics v-if="pokemonData" :pokemon="pokemonData" @save-pokemon="savePokemon" />

    <h2>My Pokedex</h2>
    <ul>
      <li v-for="(pokemon, index) in pokedex" :key="pokemon.name">
        <span @click="viewPokemonDetails(pokemon)" class="clickable">{{ pokemon.name | capitalize }}</span>
        <button @click="removePokemon(index)">Remove</button>
      </li>
    </ul>

    <!-- Mostra i dettagli del Pokemon selezionato -->
    <div v-if="selectedPokemon" class="pokemon-details">
      <h3>{{ selectedPokemon.name | capitalize }}</h3>
      <img :src="selectedPokemon.sprites.front_default" alt="Pokemon image" />
      <p>Height: {{ selectedPokemon.height }}</p>
      <p>Weight: {{ selectedPokemon.weight }}</p>
    </div>
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
      selectedPokemon: null, // Pokemon selezionato per visualizzare le caratteristiche
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
      if (!this.pokedex.some(p => p.name === pokemon.name)) {
        this.pokedex.push(pokemon);
        this.updateLocalStorage();
      }
    },
    removePokemon(index) {
      this.pokedex.splice(index, 1);
      this.updateLocalStorage();
    },
    viewPokemonCharacteristics(pokemon) {
      this.selectedPokemon = pokemon; // Visualizza il Pokemon selezionato
    },
    updateLocalStorage() {
      localStorage.setItem('pokedex', JSON.stringify(this.pokedex));
    },
    loadFromLocalStorage() {
      const savedPokedex = localStorage.getItem('pokedex');
      if (savedPokedex) {
        this.pokedex = JSON.parse(savedPokedex);
      }
    },
  },
  mounted() {
    this.loadFromLocalStorage(); // Carica i dati salvati al caricamento del componente
  },
};
</script>

<style>
.clickable {
  cursor: pointer;
  color: blue;
  text-decoration: underline;
}

.pokemon-details {
  margin-top: 20px;
}
</style>