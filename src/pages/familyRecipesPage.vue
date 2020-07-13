<template>
  <div class="container">
    <b-row>
      <b-col>
   
        <familyRecipePreviewList title="Family Recipes"  :familyArrayList="familyArray"  />
          
      </b-col>

     
    </b-row>

  </div>
</template>

<script>
import familyRecipePreviewList from "../components/FamilyRecipesPreviewList";


export default {

    data() {
    return {
      // image_load: false,
     familyArray: {
      type: [],
      required: true
    }
    };
  },
  
  components: {
    familyRecipePreviewList,
  
  },created(){
    this.getFamilyRecipes()
    

  }
  ,   methods: {
    async getFamilyRecipes() {
      if(this.$root.store.username){
      try {
        const response = await this.axios.get(
          "http://localhost:3000/users//myFamilyRecepies/getPreview"
        );

       console.log(response)
               const infoArray=response.data
               this.familyArray = [];
              this.familyArray.push(...infoArray);

      } catch (error) {
        console.log(error);
      }
    }
    }
   }
  
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
