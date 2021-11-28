<template>
<b-container>
  <b-card
    class="mb-4">
     <b-card-text>
      <img :src="currentImg" alt="Pokemon">
      <h3 class="title">{{ upperFirstLetter }}</h3>
      <div class="size">
        <strong>Size:</strong>
        <span><strong>Height:</strong> {{pokemon.height}}</span>
        <span><strong>Weight:</strong> {{pokemon.weight}}</span>
      </div>
      <div class="abilities info">
        <strong>Abilities:</strong>
        <button v-for="(value, index) in pokemon.abilities" :key="index">
        {{value.name}}
        </button>
      </div>
      <div class="types info">
        <strong>Types:</strong>
        <button v-for="(value, index) in pokemon.types" :key="index">
        {{value}}
        </button>
      </div>
      

      <div class="options">
        <b-button @click="setSprite" class="w-100 mt-2 btn" variant="dark">
          Change Sprite
        </b-button>
        <router-link to="/">
          <b-button class="w-100" variant="outline-dark">
            Home
          </b-button>
        </router-link>
      </div>
    </b-card-text>
  </b-card>
</b-container>
  
</template>

<script>
export default {
  data(){
    return{
      isFront: true,
      currentImg: '',
      data: {},
      pokemon: {
        name: '',
        abilities: [],
        types: [],
        front: '',
        back: '',
        height: 0,
        weight: 0,
      }
    }
   
  },
  created: function(){
    this.data = this.$route.params.data;
    this.pokemon.name = this.data.name;
    this.pokemon.front = this.data.sprites.front_default;
    this.pokemon.back = this.data.sprites.back_default;
    this.pokemon.height = this.data.height;
    this.pokemon.weight = this.data.weight;
    this.data.types.forEach(type => {
      this.pokemon.types.push(type.type.name);
    });

    this.data.abilities.forEach(index => {
      this.pokemon.abilities.push(index.ability);
    });
    this.currentImg = this.pokemon.front;
  },
  computed: {
    upperFirstLetter: function(){
      let newName = this.pokemon.name[0].toUpperCase() + this.pokemon.name.slice(1);
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
}
</script>

<style scoped>
  .title{
    font-weight: 600;
    font-size: 2.3rem;
    padding-bottom: 1rem;
  }
  .abilities{
    margin-top: 1rem; 
  }

  .info + .info{
    margin-top: 0.8rem;
  }

  .types button,
  .abilities button,
  .size span{
    border: none;
    font-weight: 500;
    border-radius: 2rem;
    padding: 0.4em 0.8rem;
    margin-left: 0.5rem;
  }

  .types button{
    background: red;
    color: #fff;
  }

  .abilities button{
    background: purple;
    color: #fff;
  }

  .size span{
    background: blue;
    color: #fff;
  }

  .options{
    margin-top: 2rem;
  }
  .btn{
    margin-bottom: 0.5rem;
  }
</style>
