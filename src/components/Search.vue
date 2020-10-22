<template>
    <div class="searchbar">
    <form @submit.prevent="setPokemonUrl">
      <input type="text" v-model="searchvalue" placeholder="Buscar Pokemon">
    </form>
    </div>
</template>
<script>
export default {
    props:[
        'apiUrl',
        'color',
        'habitat'
    ],
    data:()=>{
        return{
            searchvalue:''
        }
    },
    methods:{
        setPokemonUrl(){
            if(this.searchvalue !== ''){
                this.$emit('setPokemonUrl', {url1: this.apiUrl + 'pokemon/' + this.searchvalue,
                 url2:this.apiUrl + 'pokemon-species/' + this.searchvalue});
            }
        }
        
    }
}
</script>

<style lang="scss" scoped>
  .searchbar {
    position:relative;
    float: right;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;

    input {
      text-align: center;
      border: none;
      outline: none;
      border-radius: 5px;
      padding: 10px 40px 10px 10px;
      width: calc(100% - 50px);
      font-size: 1rem;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);
    }

    i {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 1.25rem;
      color: #0A2E50;
      cursor: pointer;
    }
  }
</style>