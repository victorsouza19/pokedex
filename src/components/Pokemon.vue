<template>
  <b-card
    :img-src="currentImg"
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-4">
    <b-card-text>
      <!-- {{num}}  -->
      <h3>{{ upperFirstLetter }}</h3>
      <p>{{ pokemon.type }}</p>
      <b-button @click="setSprite" class="w-100" variant="outline-dark">Change Sprite</b-button>
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
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;

      console.log(this.pokemon);

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