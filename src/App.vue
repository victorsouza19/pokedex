<template>
  <div id="app">
    <b-container>
      <img class="pokemon-logo" src="./assets/pokemon-logo.png" alt="POKEMON">
      <br>
      <div class="input-group mt-5 mb-4 d-flex justify-content-center">
        <input type="text" class="search" placeholder="Find Pokemons by name!" aria-label="Find Pokemons by name!" v-model="search">
        <div class="input-group-append">
          <button @click="searchPokemons" class="btn-search btn" type="button"><font-awesome-icon class="icon" icon="search" /></button>
        </div>
      </div>

      <b-row cols="3" class="poke-div">
        <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
          <Pokemon 
            :num="index+1"
            :name="poke.name" 
            :url="poke.url"
          />
        </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  data(){
    return{
      search: '',
      filteredPokemons: [],
      pokemons: []
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?offset=0&limit=151")
      .then(res => {
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;

      }).catch(err => {
        console.log(err);
    });
  },
  components: {
    Pokemon
  },
  methods: {
    searchPokemons: function(){
      this.filteredPokemons = this.pokemons;

      if(this.search == '' || this.search == ' '){
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toLowerCase()
        .includes(this.search.toLowerCase()));
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: #1c2733;
  padding-top: 5rem;

  min-height: 100vh;
}

.pokemon-logo{
  max-width: 50%;
}

.search{
  border: 2px solid rgb(60, 127, 214);
  padding: 0.4rem 1rem;
  border-radius: 2rem;
}

.btn-search{
  border-radius: 0 2rem 2rem 0 !important;
  border: 2px solid rgb(60, 127, 214) !important;
  border-left: none !important;
  background-color: #fff !important;
}

.icon{
  margin: 0 0.3rem 0 0;
}

</style>
