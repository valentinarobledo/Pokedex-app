<template>
  <div id="app">  
    <div class="container">
      <div class="row">
        <img class="banner" src="./banner.png" alt="logo">
      </div>
      <div class="row">
        <div class="col-sm-4">
          lista
        </div>
        <div class="col-sm-8">
          <Search
          :apiUrl="apiUrl" 
          @setPokemonUrl="setPokemonUrl"/>
          <List 
          :imgUrl="imgUrl" 
          :apiUrl="apiUrl"
          @setPokemonUrl="setPokemonUrl"/>
          <Details
          v-if="showDetails"
          :pokemonUrl="pokemonUrl"
          :imgUrl="imgUrl"
          @closeDetails="closeDetails"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'
import Search from './components/Search.vue'
import Details from './components/Details.vue'

export default {
  name: 'App',
  data(){
    return{
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      imgUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
      pokemonUrl:'',
      showDetails:false
    }
  },
  components: {
    List,
    Search,
    Details
  },
  mounted(){

  },
  methods:{
    setPokemonUrl(url){
      this.pokemonUrl=url;
      this.showDetails=true;
    },
    closeDetails(){
      this.pokemonUrl='';
      this.showdetails=false;
    }
  }
}
</script>

<style lang="scss">
@import url('https://use.fontawesome.com/releases/v5.8.2/css/all.css');

.banner{
  margin-top:30px;
}
</style>
