<template>
    <div class="app">
        <div class="title-pp">
            <h4>Filters</h4>
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Type</div>
            </div>
            <div class="row">
                <div class="type" v-for="(type, index) in types" v-bind:key="index">
                    <b-form-checkbox v-model="types[index].right" >{{ type.name }}</b-form-checkbox>
                </div>    
            </div>            
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Color</div>
            </div>
            <div class="row">
                <div class="color" v-for="(color, index) in colors" v-bind:key="index">
                    <b-form-checkbox v-model="colors[index].right">{{ color.name }}</b-form-checkbox>
                 </div>
            </div>
        </div>
        <div class="filter">
            <div class="row">
                <div class="title">Gender</div>
            </div>
                <div class="gender" v-for="(gender, index) in genders" v-bind:key="index">
                    <b-form-checkbox v-model="genders[index].right">{{ gender.name }}</b-form-checkbox>
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
    font-size:1em;
    font-weight: bolder;
    background-color:#4caf50;
    border-radius:2em;
    width:5em;
    text-align: center;
    margin-bottom:0.5em;
}
.title-pp{
    margin-top:1em;
    text-decoration: underline;
    color:#1bb120;
}
.filter{
    margin-top:2em;
}
.color, .type{
    width:150px;
}
</style>