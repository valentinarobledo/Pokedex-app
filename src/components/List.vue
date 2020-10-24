<template>
  <div id="app">  
    <div class="container">
      <div v-for="(pokemon, index) in pokemons" v-bind:key="'poke'+index"  @click="setPokemonUrl(pokemon.url, pokemon.id)">
				<b-card
				:img-src="imgUrl + pokemon.id + '.png'"
				img-alt="Image"
				img-top
				tag="article"
				style="max-width: 12rem;"
				class="mb-2 card"
			>
				<b-card-text>
          <div class="text">
					<h5>{{ pokemon.name }}</h5>
          </div>
				</b-card-text>

			</b-card>
		</div>
		<div id="scroll-trigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
    </div>
  </div>
</template>

<script>
 export default {
    props: [
      'imgUrl',
      'apiUrl'
    ],
    data: () => {
      return {
        pokemons: [],
        nextUrl: '',
        currentUrl: ''
      }
    },
    methods: {
      fetchData() {
        let req = new Request(this.currentUrl + 'pokemon/');
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.nextUrl = data.next;
            data.results.forEach(pokemon => {
              pokemon.id = pokemon.url.split('/')
                .filter(function(part) { return !!part }).pop();
              this.pokemons.push(pokemon);
            });
          })
          .catch((error) => {
            console.log(error);
          })
      },
      scrollTrigger() {
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if(entry.intersectionRatio > 0 && this.nextUrl) {
              this.next();
            }
          });
        });

        observer.observe(this.$refs.infinitescrolltrigger);
      },
      next() {
        this.currentUrl = this.nextUrl;
        this.fetchData();
      },
      setPokemonUrl(url, id) {
        this.$emit('setPokemonUrl', {url1:url, url2:this.apiUrl + 'pokemon-species/' + id});
      }
    },
    created() {
      this.currentUrl = this.apiUrl;
      this.fetchData();
    },
    mounted() {
      this.scrollTrigger();
    }
  }
</script>

<style lang="scss">
.container
{
	display: flex;
	flex-wrap: wrap;
}
.mb-2
{
	text-align: center;
	color:white;
}
.card
{
	margin:20px;
	border-radius: 5px;
	cursor: pointer;
	box-shadow: 0 15px 30px rgba(0,0,0,.2),
							0 10px 10px rgba(0,0,0,.2);
  &:hover{
    transform:scale(1.3);
    border-radius:15em;
  }
}
.text
{
  background-color:#6b6969;
  border-radius:10px;
  &:hover{
    background-color:#9c9898;
  }
}
h5
{
  font-weight:900 !important;
}
</style>