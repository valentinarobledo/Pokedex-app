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
          :color="color"
          :habitat="habitat"
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
      apiUrl: 'https://pokeapi.co/api/v2/',
      imgUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
      color:'',
      habitat:'',
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
      this.pokemonUrl=url.url1;
      this.fetchData(url.url2);
      this.showDetails=true;     
    },
    fetchData(url) {
      let req = new Request(url);
      fetch(req)
        .then((resp) => {
          if(resp.status === 200)
            return resp.json();
        })
        .then((data) => {
          this.color=data.color.name;
          console.log(data.color.name);
          console.log(data.habitat.name);
          this.habitat=data.habitat.name;
        })
        .catch((error) => {
          console.log(error);
        })
      },
    closeDetails(){
      this.pokemonUrl='';
      this.showDetails=false;
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
