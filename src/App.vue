<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/poke.png" alt="">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text">
      <div v-for="(poke, index) in pokemons" :key="index">
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
      pokemons:[]
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=152&offset=0").then(res => {
      console.log(res.data.results)
      this.pokemons = res.data.results
    })
  },
  components: {
    Pokemon
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
</style>
