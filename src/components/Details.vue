<template>
	<div class="details">
			<div class="view" v-if="show">
				<div class="row">
					<div class="col-sm-4">
							<div v-if="pokemon" class="image">
									<img :src="imgUrl + pokemon.id + '.png'" alt="" width="250" height="250">
							</div>
					</div>
					<div class="col-sm-8">
						<div v-if="pokemon" class="data">
							<h2>{{pokemon.name}} - {{pokemon.id}}</h2>
							<div class="prop">
								<div class="left">Height</div>
								<div class="right">{{pokemon.height}}</div>
							</div>
							<div class="prop">
								<div class="left">Weight</div>
								<div class="right">{{pokemon.weight}}</div>
							</div>
							<div class="prop">
								<div class="left">Category</div>
								<div class="right"></div>
							</div>
							<div class="prop">
								<div class="left">Gender</div>
								<div class="right"></div>
							</div>
							<div class="prop">
								<div class="left">Habitat</div>
								<div class="right"></div>
							</div>
							<div class="prop">
								<div class="left">Color</div>
								<div class="right"></div>
							</div>
						</div>
					</div>										
				</div>
			</div>
			<h2 v-else>The pokemon was not found</h2>
      <button class="close" v-on:click="closeDetails">close</button>
	</div>
</template>
<script>
  export default {
    props: [
      'pokemonUrl',
      'imgUrl'
    ],
    data: () => {
      return {
        show: false,
        pokemon: {}
      }      
    },
    methods: {
      fetchData() {
        let req = new Request(this.pokemonUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.pokemon = data;
            this.show = true;
          })
          .catch((error) => {
            console.log(error);
          })
      },
      closeDetails() {
        this.$emit('closeDetails');
      }
    },
    created() {
      this.fetchData();
    }
  }
</script>

<style lang="scss">
.details{
    display: flex;
    justify-content: center;
    position: fixed;
    top: 0;
    left: 0;
    padding: 60px 10px 10px;
    width: calc(100% - 20px);
    height: calc(100vh - 20px);
    background: rgba($color: #000000, $alpha: .7);

		.view{
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative;
      width: 100%;
      max-width: 600px;
      padding: 50px 0 0;
      background-color: #fff;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);
    
		.data{
			display: flex;
			justify-content: flex-start;
			align-items: center;
			flex-direction: column;
			padding-left:22%;
			width: 100%;
			margin-bottom: 40px;	
			
        .prop{
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          margin-bottom: 10px;

          .left { float: left; }
          .right { float: right; }
        }		
		}
		}
}
</style>