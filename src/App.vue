<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/poke.png" alt="">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" placeholder="Buscar pokemon" v-model="busca" class="input is-rounded">
      <button  class="button  is-fullwidth" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-if="mostrarErro" class="mensagem-erro">
        {{ mensagemErro }}
      </div>
      <div class="pokemon-list" v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index +1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Pokemon from './components/PokemonList.vue'

export default {
  name: 'App',
  data(){
    return {
      pokemons:[],
      filteredPokemons: [],
      busca: '',
      mostrarErro: false,
      mensagemErro: '' 
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=152&offset=0").then(res => {
      console.log(res.data.results)
      this.pokemons = res.data.results
      this.filteredPokemons = res.data.results
    })
  },
  components: {
    Pokemon
  },
  methods:{
    buscar: function(){
      this.mensagemErro = '';

      this.filteredPokemons = this.pokemons

      if(this.busca.trim() === ''){
        this.filteredPokemons = this.pokemons
      }else{
        const pokemonEncontrado = this.pokemons.find(pokemon => pokemon.name.toLowerCase() === this.busca.toLowerCase())
        if (!pokemonEncontrado){
          this.mostrarErro = true
          this.mensagemErro = 'Pokemon não encontrado ou não pertence a essa base de dados'
        }else{
          this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(this.busca.toLowerCase()));
        }
      }
    },
  },
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

#buscaBtn{
  margin-top: 2%;
  border-radius: 30px;
  background-color: #4169E1;
  color: white;
}

.mensagem-erro{
  color: red;
}


</style>
