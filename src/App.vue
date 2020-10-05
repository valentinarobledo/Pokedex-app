<template>
  <div id="app">  
    <div class="container">
     
     <div>
  <b-navbar class="nav" type="dark">
    <b-navbar-brand href="#">POKEDEX</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form>
      
      </b-navbar-nav>
  </b-navbar>
</div>
       <div class="pokemones">
          <div v-for="pokemon in pokemons" v-bind:key="pokemon.id">
            <b-card
              img-src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/1.png"
              img-alt="Image"
              img-top
              tag="article"
              style="max-width: 20rem;"
              class="mb-2"
            >
              <b-card-text class="name">
                {{ pokemon.name }}
              </b-card-text>
            </b-card>
        </div>    
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return{
      pokemons:null
    }
  },
  mounted(){
    this.getpokemons();
  },
  methods:{
    getpokemons(){
      console.log('codigo')
      axios
        .get('https://pokeapi.co/api/v2/pokemon/')
          .then( response => {
                console.log(response)
                this.pokemons = response.data.results
            })
          .catch( e=> console.log(e))
    }
  }
}
</script>

<style lang="scss">
.nav{
  margin:15px;
  background-color: #80c19dc9 !important;
  border-radius: 20px;
}
.title
{
  text-align: center;
  margin-top:50px;
  margin-bottom:20px;
}
.pokemones{
  margin-top:20px;
  margin-bottom: 15px;
  min-height: 100vh;
  display: flex;
  flex-wrap: wrap;
}
.name{
  color:#717579;
  font-size:20px;
  text-align:center;
  font-weight: bolder;
}
.mb-2{
  width:200px;
  margin:8px;
  border-radius:10px !important;
  &:hover{
    border-radius:500px !important;
    background:#d4d7daab
  }
}
</style>
