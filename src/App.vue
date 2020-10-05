<template>
  <div id="app">  
    <div class="container">
      <List 
      :imgUrl="imgUrl" 
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"/>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'

export default {
  name: 'App',
  data(){
    return{
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      imgUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
      pokemonUrl:''
    }
  },
  components: {
    List
  },
  mounted(){

  },
  methods:{
    setPokemonUrl(url){
      this.pokemonUrl=url;
    }
  }
}
</script>

<style lang="scss">
</style>
