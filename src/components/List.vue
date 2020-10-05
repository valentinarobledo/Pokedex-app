<template>
  <div id="app">  
    <div class="container">
      <div v-for="(pokemon, index) in pokemons" v-bind:key="'poke'+index">
				<b-card
				:img-src="imgUrl + pokemon.id + '.png'"
				img-alt="Image"
				img-top
				tag="article"
				style="max-width: 15rem;"
				class="mb-2"
			>
				<b-card-text>
					<h5>{{ pokemon.name }}</h5>
				</b-card-text>

			</b-card>
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
        let req = new Request(this.currentUrl);
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
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
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
	color:#777474;
}
</style>