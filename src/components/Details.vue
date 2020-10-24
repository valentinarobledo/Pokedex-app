<template>
	<div class="details">
			<div class="view" v-if="show">
				<div class="row">
					<div class="col-sm-4">
						<div v-if="pokemon" class="image">
							<img :src="imgUrl + pokemon.id + '.png'" alt="" width="250" height="250">
						</div>
						<div class="evolution">
							<h5>Evolution from </h5>
							<div class="item">{{evolution}}</div>
						</div>
					</div>
					<div class="col-sm-8">
						<div v-if="pokemon" class="data">
							<h2>{{pokemon.name}} <i class="fas fa-paw"></i></h2>
							<div class="id">{{pokemon.id}}</div>
							<div class="prop">
								<div class="left">Height</div>
								<div class="right">{{pokemon.height}}</div>
							</div>
							<div class="prop">
								<div class="left">Weight</div>
								<div class="right">{{pokemon.weight}}</div>
							</div>
							<div class="prop">
								<div class="left">Habitat</div>
								<div class="right">{{habitat}}</div>
							</div>
							<div class="prop">
								<div class="left">Color</div>
								<div class="right">{{color}}</div>
							</div>
							<h4>Pokemon Types <i class="fas fa-feather-alt"></i></h4>
							<div class="types">
								<div class="type" 
									v-for="(value, index) in pokemon.types"
									:key="'value'+index">
									{{ value.type.name }}
								</div>
							</div>							
						</div>
					</div>										
				</div>
      <button class="close" @click="closeDetails">close</button>				
			</div>
			<h2 v-else>The pokemon was not found</h2>
	</div>
</template>
<script>
  export default {
    props: [
			'pokemonUrl',
			'imgUrl',
			'color',
			'habitat',
			'evolution'
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
      max-width: 700px;
      padding: 50px 0 0;
      background-color: #fff;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);

			.image{
				background:#314148;
				width: 250px;
				border-radius:250px;
			}
			.evolution{
				margin-top:45px;
			}
			.item{
				text-align: center;
				font-size:1em;
				text-transform: capitalize;
				letter-spacing: 1.5px;
				background-color:#2b70c9;
				border-radius:20px;
				color:white;
				font-weight: 600;
				cursor:pointer;
				&:hover{
					background-color:#1899d6;
				}
			}
			.data{
				display: flex;
				justify-content: flex-start;
				align-items: center;
				flex-direction: column;
				padding-left:5em;
				width: 100%;
				margin-bottom: 40px;	
				
					.prop{
						width: 90%;
						max-width: 400px;
						border-bottom: 1px solid #ccc;
						margin-bottom: 10px;

						.left { float: left; }
						.right { float: right; font-weight: 600; text-transform: capitalize; }
					}
					h2, h4, h5{
						&:hover{
							text-decoration:underline;
							cursor:pointer;
						}
					}		
        .types{
          display: flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          width: 90%;
          max-width: 400px;

          .type{
            margin: 0 10px 10px 0;
			text-align: center;
            padding: 5px 10px;
            border-radius: 20px;
            color: #fff;
            font-size: 1rem;
            letter-spacing: 1.5px;
            text-transform: capitalize;
            word-wrap: none;
            word-break: keep-all;
			background-color:#ea2b2b;
			&:hover{
					background-color:#ff7878;
			}
          }
        }		
				h2{
					font-weight:900;
					text-transform: capitalize;
				}
				h4{
					margin-bottom:1em;
					margin-top:1.5em;
					font-weight:600;
				}
				h5{
					font-weight: 600;
				}		
				.id{
					width:50px;
					text-align:center;
					color:white;
					font-size:18px;
					font-weight: 900;
					background-color:#ffb100;
					border-radius:16px;
					margin-bottom:1em;
					&:hover{
						background-color:#ffc775;
					}
				}	
			}
      .close {
        outline: none;
        border: none;
        border-radius: 5px;
        background-color:#4b4b4b;
        color: #efefef;
        padding: 10px 20px;
        margin-bottom: 20px;
        font-size: 1.2rem;
        cursor: pointer;
      }
		}
}
</style>