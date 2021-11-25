<template>
  <div id="app">
    <b-container>
      <b-row cols="3" class="poke-div">
        <div v-for="(poke, index) in pokemons" :key="index">
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
      pokemons: []
    }
  },

  components: {
    Pokemon
  },

  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?offset=0&limit=151")
      .then(res => {
        this.pokemons = res.data.results;

      }).catch(err => {
        console.log(err);
    });
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
  background: #000;
  padding-top: 5rem;
}

</style>
