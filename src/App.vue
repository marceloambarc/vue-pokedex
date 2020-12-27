<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img id="img" src="./assets/arc.png" alt="Arcturo Logo">
      <hr>
      <h3 class="is-seize-4">Pokédex</h3>
      <input class="input is-rounded" type="text" placeholder="Buscar Pokémon" v-model="search">
      <button id="searchBtn" class="button is-success" @click="find">Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      search: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods: {
    find: function(){
      this.filteredPokemons = this.pokemons;
      if(this.search == '' || this.search == ' '){
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.search);
      }
    }
  },
  computed: {
    /*
    searchResults: function(){
      if(this.search == '' || this.search == ' '){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.search);
      }
    }
    */
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
  margin-top: 60px;
}

#img {
  max-height: 200px;
}

#searchBtn {
  margin-top: 2%;
}
</style>
