<template>
  <div id="app">  
    <div class="container">
      <div class="row">
        <img class="banner" src="./banner.png" alt="logo">
      </div>
      <div class="row">
        <div class="col-sm-4">
          <Filt
          :apiUrl="apiUrl" 
          @setPokemonUrl="setPokemonUrl"
          @filter="filter"/>
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
import Filt from './components/Filt.vue'
import Search from './components/Search.vue'
import Details from './components/Details.vue'
import axios from "axios";

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
    Filt,
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
    filter(obj){
      var name;
      if(obj.type=='gender'){
        obj.val.forEach(element => {
          if(element.right){
            name=element.name;
            this.fetchData2('gender/' + name);
          }
        });
        
      }
      else if(obj.type=='type'){
        this.fetchData2('type/' + name);
      }
      else{
        this.fetchData2('pokemon-color' + name);
      }
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
          this.habitat=data.habitat.name;
        })
        .catch((error) => {
          console.log(error);
        });
      },
    fetchData2(url) {
      //let req = new Request(url);
      /*fetch(req)
        .then((resp) => {
          if(resp.status === 200)
          console.log(resp.json());
            return resp.json();

        })
        .then((data) => {
          console.log(data);
          return data;
        })
        .catch((error) => {
          console.log(error);
        });*/
      axios.get(url)
      .then(function(data){
        console.log(data);
      })
      .catch((error)=>{
        console.log(error);
      });
        
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
