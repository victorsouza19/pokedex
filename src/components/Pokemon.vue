<template>
  <b-card
    style="max-width: 20rem;"
    class="mb-4">
    <b-card-text>
      <img :src="currentImg" alt="Pokemon">
      <h3>{{ upperFirstLetter }}</h3>

      <router-link :to="{name: 'Pokemon', params: {name: this.name, data: this.pokemonData}}">
        <b-button class="w-100" variant="dark">
          Show details
        </b-button>
      </router-link>

      <b-button @click="setSprite" class="w-100 mt-2" variant="outline-dark">
        Change Sprite
      </b-button>

    </b-card-text>
  </b-card>

</template>

<script>
import axios from 'axios';

export default {
  data(){
    return{
      isFront: true,
      currentImg: '',
      pokemonData: {},
      pokemon: {
        type: '',
        front: '',
        back: ''
      },
    }
  },
  props: {
    num: Number,
    name: String,
    url: String
  },
  computed: {
    upperFirstLetter: function(){
      let newName = this.name[0].toUpperCase() + this.name.slice(1);
      return newName;
      
    }
  },
  methods: {
    setSprite: function(){
      if(this.isFront){
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      }else{
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    }
  },
  created: function(){
    axios.get(this.url).then(res => {
      this.pokemonData = res.data;
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    }).catch(err => {
      console.log(err);
    })
  }

}
</script>

<style scoped>
  h3{
    font-weight: 600;
    margin-bottom: 2rem;
  }
</style>