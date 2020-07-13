<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <br>
     <router-link :to="{ name: 'myFavorites' }">favorite recipes Page</router-link>
     <router-link :to="{ name: 'main' }">Your recipes Page</router-link>|
    <SearchForm v-on:move-query="SerchForResults" />
      <br>
      <br>
      <button @click="sortRecipesByDuration">sort by duration</button>
      <button @click="sortRecipesByPopularity">sort by popularity</button>
      <br>
      <br>
      <b-alert variant="danger" dismissible fade :show="showDismissibleAlert"  @dismissed="showDismissibleAlert=false">
      There is no results, try another search words
    </b-alert>
      <recipePreviewList class="recipePreview" :recipes="recipes" title='Your search results'></recipePreviewList>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList.vue";
import SearchForm from "../components/SearchForm"
export default {
  name: "SearchPage",
  components:{
    SearchForm,
    RecipePreviewList
  },
  data(){
    return{
      recipes: [],
      isReadyToShow: true,
      showDismissibleAlert: false
    }
  },
  mounted(){
    this.loadSearchHistory();
  },
  methods:{
       loadSearchHistory(){
          try {
            if(this.$root.store.username){
              if(localStorage.lastSearch){
                this.recipes=JSON.parse(localStorage.lastSearch);
              }
            }
            else{
               if(localStorage.lastSearch){
                localStorage.removeItem('lastSearch');
              }
            }
          } catch (error) {
            
          }
       },
       async updateRecipes(query, num, cuisine, diet, intolerence) {
      try {
        let amount = num;
        if(amount==null){
          amount='5';
        }
        let url = "http://localhost:3000/recipes/search/query/"+query+"/amount/"+amount
        if(cuisine!=''||diet!=''||intolerence!=''){
          url=url+"?";
        }
        if(cuisine!=''){
          if(url.slice(-1)=="?"){
            url=url+"cuisine="+cuisine;
          }
          else{
            url=url+"&cuisine="+cuisine;
          }
          
        }
        if(diet!=''){
           if(url.slice(-1)=="?"){
            url=url+"diet="+diet;
           }
           else{
             url=url+"&diet="+diet;
           }
        }
        if(intolerence!=''){
          if(url.slice(-1)=="?"){
            url=url+"intolerence="+intolerence;
          }
          else{
            url=url+"&intolerence="+intolerence;
          }
        }
        const response = await this.axios.get(url );

        console.log(response);
        const recipesFromServer = response.data;
        // const recipes = [{"id":638741,"title":"Chipotle Black Bean Soup with Avocado Cream","readyInMinutes":45,"aggregateLikes":37,"vegetarian":true,"vegan":false,"glutenFree":true,"image":"https://spoonacular.com/recipeImages/638741-556x370.jpg"},{"id":639866,"title":"Coffee Cream Martinis","readyInMinutes":45,"aggregateLikes":19,"vegetarian":false,"vegan":false,"glutenFree":true,"image":"https://spoonacular.com/recipeImages/639866-556x370.jpg"},{"id":663931,"title":"Tuna Mexi Melts","readyInMinutes":45,"aggregateLikes":9,"vegetarian":false,"vegan":false,"glutenFree":false,"image":"https://spoonacular.com/recipeImages/663931-556x370.jpg"}]
        this.recipes = [];
        if(recipesFromServer!=null && recipesFromServer!=false){
            this.recipes.push(...recipesFromServer);
        }
        // localStorage.setItem("lastSearch",this.recipes);
        localStorage.setItem("lastSearch",JSON.stringify(this.recipes));
        if(this.recipes.length==0){
          this.showDismissibleAlert=true;
        }
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
    SerchForResults(query, num, cuisine, diet, intolerence){
      this.updateRecipes(query, num, cuisine, diet, intolerence);
      //removeRecipes();
          this.isReadyToShow=true;
    },
    removeRecipes(){
      isReadyToShow=false;
    },
    sortRecipesByDuration(){
      let tempRecipes=[];
      tempRecipes=this.recipes;
      tempRecipes.sort(function(a,b){
        return a.readyInMinutes-b.readyInMinutes;
      })
      this.recipes=[];
      this.recipes.push(...tempRecipes);
    },
    sortRecipesByPopularity(){
      let tempRecipes=[];
      tempRecipes=this.recipes;
      tempRecipes.sort(function(a,b){
        return a.aggregateLikes-b.aggregateLikes;
      })
      this.recipes=[];
      this.recipes.push(...tempRecipes);
    }
  
  }

}
</script>

<style>

</style>