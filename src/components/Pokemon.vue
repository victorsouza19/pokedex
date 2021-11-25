<template>
  <b-card
    :img-src="pokemon.front"
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-4">
    <b-card-text>
      <h3>{{num}} {{ upperFirstLetter }}</h3>
      <p>{{ pokemon.type }}</p>
    </b-card-text>

    <!-- <b-button href="#" variant="primary">More details</b-button> -->
  </b-card>

</template>

<script>
import axios from 'axios';

export default {
  data(){
    return{
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
  created: function(){
    axios.get(this.url).then(res => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      console.log(this.pokemon)

    }).catch(err => {
      console.log(err);
    })
  }

}
</script>

<style scoped>
  h3{
    font-weight: 600;
  }
</style>