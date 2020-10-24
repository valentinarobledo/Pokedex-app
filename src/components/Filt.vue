<template>
    <div class="app">
        <div class="row">
            <div class="title-pp">
                <h4>Filters</h4>
                <img class="rotated" src="../pokeball.png" width="60" alt="logo"> 
            </div>
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Type <i class="fab fa-hotjar"></i></div>
            </div>
            <div class="row">
                <div class="type" v-for="(type, index) in types" v-bind:key="index">
                    <b-form-checkbox size="lg" v-model="types[index].right" >{{ type.name }}</b-form-checkbox>
                </div>    
            </div>            
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Color <i class="fas fa-atom"></i></div>
            </div>
            <div class="row">
                <div class="color" v-for="(color, index) in colors" v-bind:key="index">
                    <b-form-checkbox size="lg" v-model="colors[index].right">{{ color.name }}</b-form-checkbox>
                 </div>
            </div>
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Gender <i class="fas fa-bolt"></i></div>
            </div>
                <div class="gender" v-for="(gender, index) in genders" v-bind:key="index">
                    <b-form-checkbox size="lg" v-model="genders[index].right">{{ gender.name }}</b-form-checkbox>
                </div>
        </div>
    </div>
</template>


<script>
 export default {
    props: [
      'apiUrl'
    ],
    data: () => {
      return {
        types:[],
        colors:[],
        genders:[]
      }
    },
    methods: {
      fetchData(url) {
        let req = new Request(this.apiUrl + url);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200){
               return resp.json();
            }
          })
          .then((data) => {
                if(url=='type/'){
                    this.types=data.results;
                }
                else if(url == 'pokemon-color/'){
                    this.colors=data.results;
                }
                else {
                    this.genders=data.results;
                }
        })
      }
  },
    created() {
      this.fetchData('type/');
      this.fetchData('pokemon-color/');
      this.fetchData('gender/');
    },
     watch:{
        genders: function(val){
            this.$emit('filter', {type:'gender', val:val});
        },
        types: function(val){
            this.$emit('filter', { type:'type', val:val});

            
        },
        colors: function(val){
            this.$emit('filter', {type: 'color', val:val});

        }

 }
 }
</script>

<style lang="scss">
.title{
    color:white;
    font-size:1.2em;
    font-weight: bolder;
    background-color:chartreuse;
    border-radius:2em;
    width:15em;
    text-align: center;
    margin-bottom:0.5em;

}
.title-pp{
    text-decoration: underline;
    color:#58cc02;
}
.filter{
    margin-top:1.5em;
}
.color, .type{
    width:150px;
}
.custom-checkbox{
    z-index:0 !important;
}
.custom-checkbox .custom-control-input:checked ~ .custom-control-label::after{
    background-color:chartreuse;
    border-color:chartreuse;
}
.rotated{
    z-index: 1000;
    margin-top: -65px;
    margin-left: 66px;
	animation-name: rotation;
	animation-duration: 5s;
	animation-iteration-count: infinite;
	animation-timing-function: linear;
}

@keyframes rotation {
  0% {
    transform:rotate(0deg);
  }
  100% {
    transform:rotate(360deg);
  }
}
</style>