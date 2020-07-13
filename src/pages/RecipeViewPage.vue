<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1> 
      <img :src="recipe.image" class="center" />
      </div>



              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>{{ recipe.aggregateLikes }} likes</div>
              <div>Vegan: {{ recipe.vegan }} </div>
              <div>vegetarian: {{ recipe.vegetarian }} </div>
              <div>glutenFree: {{ recipe.glutenFree }} </div>
              <div>servings: {{ recipe.servings }} </div>
              <div class="card">
                <div class="card-header">
                  Ingredients
                </div>
                <div class="card-body">
                  <blockquote class="blockquote mb-0">
                  <Ingredients :ingredients="recipe.Ingredients"/>            
                  </blockquote>
                </div>
              </div>

              <div class="card">
                <div class="card-header">
                  Instructions
                </div>
                <div class="card-body">
                  <blockquote class="blockquote mb-0">
                <Instructions :instructions="recipe.instructions"/>
                  </blockquote>
                </div>
              </div>



           <span>
            
             <!-- Ingredients:
            <ul>
              <li
                v-for="(r, index) in recipe.Ingredients"
                :key="index + '_' + r.id"
              >
                {{ r.name_and_amount }}
              </li>
            </ul> -->
            
            </span>
      
      <!-- </div> -->
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    <!-- </div> -->
  </div>
  </div>
</template>

<script>

import Ingredients from "../components/Ingredients.vue"
import Instructions from "../components/Instructions.vue"
export default {
   components: {
     Ingredients,
     Instructions
  },
  data() {
    return {
      recipe: null
    
    };
  },
  
  
  async created(){
    this.created()

  }, 
   methods: {


     async created() {
    try {
      let response;
      // response = [{"id":638741,"title":"Chipotle Black Bean Soup with Avocado Cream","readyInMinutes":45,"aggregateLikes":37,"vegetarian":true,"vegan":false,"glutenFree":true,"image":"https://spoonacular.com/recipeImages/638741-556x370.jpg","Ingredients":[{"name_and_amount":"1 small ripe avocado, pitted, peeled and cut into cubes (about 1/2 cup)"},{"name_and_amount":"3 cans (about 15 ounces each) black beans, rinsed and drained"},{"name_and_amount":"4 large carrots, peeled and diced (about 2 cups)"},{"name_and_amount":"4 cups Swanson® Chicken Broth (Regular, Natural Goodness® or Certified Organic)"},{"name_and_amount":"1 can (7 ounces) chipotle peppers in adobo sauce, minced"},{"name_and_amount":"2 tablespoons chopped fresh cilantro leaves"},{"name_and_amount":"1 tablespoon lemon juice"},{"name_and_amount":"2 tablespoons olive oil"},{"name_and_amount":"2 large onions, diced (about 2 cups)"},{"name_and_amount":"1/4 cup sour cream"}],"instructions":"<ol><li>Heat the oil in a 4-quart saucepan. Add the carrots and onions and cook until they're tender-crisp, stirring occasionally. Add the peppers and cook and stir for 1 minute.</li><li>Stir the broth and beans into the saucepan and heat to a boil. Reduce the heat to low. Cook for 25 minutes.</li><li>Mash the avocado with a fork in a small bowl.  Stir in the sour cream, cilantro and lemon juice.</li><li>Spoon one-third of the broth mixture into a blender. Cover and blend until the mixture is smooth. Pour the mixture into a large bowl. Repeat twice more with the remaining broth mixture. Return the blended mixture to the saucepan. Cook over medium heat until the mixture is hot and bubbling.  Divide the soup among 8 serving bowls. Top with the avocado mixture.</li></ol>","servings":8}]

      try {
        response = await this.axios.get(
          "http://localhost:3000/recipes/getFullRecipe/"+this.$route.params.recipeId
        
        );
        console.log(response)
      
     

        // console.log("response.status", response.status);
        // if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

         let {
        servings,
        instructions,
        Ingredients,
        glutenFree,
        vegan,
        vegetarian,
        title,
        image,
        readyInMinutes,
        aggregateLikes,
        id
      } = response.data[0];
      console.log(  servings,
        instructions,
        Ingredients,    glutenFree,
        vegan,
        vegetarian,
        title,
        image,
        readyInMinutes,
        aggregateLikes,
        id)


      
      let _recipe = {
        servings,
        instructions,
        Ingredients,
        glutenFree,
        vegan,
        vegetarian,
        title,
        image,
        readyInMinutes,
        aggregateLikes,
        id
      };

      // var xmlString = instructions;
      // var doc = new DOMParser().parseFromString(xmlString, "text/xml");
      // console.log(doc.firstChild.innerHTML); // => <a href="#">Link...
      // console.log(doc.firstChild.firstChild.innerHTML); // => Link

      console.log(_recipe)

      this.recipe = _recipe;

      // this.recipe.id=response[0].id
      // this.recipe.aggregateLikes=response[0].aggregateLikes
      //  this.recipe.readyInMinutes=response[0].readyInMinutes
      //   this.recipe.image=response[0].image
      //    this.recipe.title=response[0].title
      //     this.recipe.vegetarian=response[0].vegetarian
      //      this.recipe.vegan=response[0].vegan
      //       this.recipe.glutenFree=response[0].glutenFree

    console.log(this.recipe)



  //insert user watched to DB
  try{
    if(this.$root.store.username){
     isInsert=await this.axios.post(
          "http://localhost:3000/users/addNewRecipeToWatched",
           {
            id: this.recipe.id
          }
        
        );
    }
  }
    
    
  catch(error){
     console.log(error);
    
  }

  try{

    addToHistory=await this.axios.post(
          "http://localhost:3000/users/addReciepeToHistory",
           {
            id: this.recipe.id
          }
        
        );
      
    }
  
  catch(error){
     console.log(error);
    
  }
  




    } catch (error) {
      console.log(error);
    }
  }
  },
  // computed:{
  //   var parser = new DomParser();
  // var parsedHtml    = parser.parseFromString(data, "text/html");
  // let pTags = parsedHtml.getElementsByTagName("ol");
  // let vocab = []
  // pTags.forEach(function(item){
  //   let word = item.getElementsByTagName("il")[0].textContent.trim();
  //   vocab.push(word)
  // });
  // }
 
  }
 
  


</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
