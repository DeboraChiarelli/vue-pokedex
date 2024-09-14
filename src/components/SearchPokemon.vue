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

<style scoped>
/* Layout base */
#app {
  background-color: #f2f2f2; /* Colore chiaro di sfondo */
  border: 2px solid #3b4cca; /* Bordo blu in stile Pokedex */
  border-radius: 15px;
  padding: 20px;
  width: 350px;
  margin: 20px auto;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
}

/* Titoli e Testi */
h1 {
  color: #ffcb05; /* Giallo in stile Pokémon */
  font-family: 'Arial', sans-serif;
  text-align: center;
}

h2 {
  color: #3b4cca; /* Colore blu in stile Pokedex */
  text-transform: uppercase;
  margin-bottom: 10px;
}

h3 {
  color: #ff0000; /* Rosso in stile Pokedex */
}

/* Input e Pulsanti */
input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 2px solid #ffcb05;
  border-radius: 5px;
  box-sizing: border-box;
}

button {
  background-color: #ff0000; /* Colore rosso pulsante */
  color: #ffffff;
  border: none;
  padding: 10px;
  width: 100%;
  border-radius: 5px;
  font-size: 14px;
  cursor: pointer;
  margin-top: 10px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #cc0000; /* Cambia colore al passaggio del mouse */
}

/* Stile della lista dei Pokémon */
ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #ffcb05;
  color: #000;
  padding: 8px;
  margin-bottom: 5px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li span {
  cursor: pointer;
}

li button {
  background-color: #cc0000;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

li button:hover {
  background-color: #ff0000;
}

/* Stile dei dettagli del Pokémon */
.pokemon-details {
  margin-top: 20px;
  background-color: #f2f2f2;
  padding: 15px;
  border-radius: 10px;
  text-align: center;
}

.pokemon-details img {
  width: 150px;
}

.pokemon-details p {
  margin: 5px 0;
}
</style>