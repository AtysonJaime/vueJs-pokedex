<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <h4 class="is-size-4"> Pokedex</h4>
      <input class='input is-rounded' type="text" placeholder="Buscar pokemon pelo nome" v-model="busca"/>
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-for="(poke,index) in filteredPokemons" :key="poke.url">
        <Pokemon :name='poke.name' :url='poke.url' :num='index + 1'/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon'

export default {
  name: 'App',
  data (){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  components: {
    Pokemon
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    });
  },
  
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if(this.busca.trim().length === 0) {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },

  computed: {
    // resultadoBusca: function(){
    //   if(this.busca.trim().length == 0) {
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca)
    //   }
    // }
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

#buscaBtn {
  margin-top: 2%;
  margin-bottom: 2%;;
}
</style>
