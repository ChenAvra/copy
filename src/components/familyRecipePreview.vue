<template>

  

    
      <div class="recipe-preview">
    <!-- <b-card  > -->
      <b-card-text>
         <h3  class="recipe-title"  >
            {{ this.FamilyRecipe.title }}

                    
         </h3>

         <div class="ownerRecipe">  {{this.FamilyRecipe.ownerRecipe}} </div>

 


          <router-link
          :to="{ name: 'familyRecipe', params: { recipeId: this.FamilyRecipe.id} }">
          <div class="recipe-body">
            <img  :src="FamilyRecipe.image" class="recipe-image" />
          </div>
          </router-link>
       
      </b-card-text>
    <!-- </b-card> -->
  </div>  
   
    
  
</template>

<script>
export default {



 
  props: {
    FamilyRecipe: {
      type: Object,
      required: true
    }

  },
    created() {
 
   
      

  },
   methods: {
    
   }
    
};
</script>

<style scoped>
/* .fa-heart-o {
  color: red;
  cursor: pointer;
}

.fa-heart {
  color: red;
  cursor: pointer;
} */
.recipe-preview {
  display: inline-block;
  width: 100%;
  height: 100%;
  position: relative;
  margin: 2px 2px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;

}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 100%;
  height: auto;
  /* -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover; */
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  /* overflow: hidden; */
}

.recipe-preview .recipe-footer .recipe-title {
  /* padding: 10px 10px; */
  width: 100%;
  font-size: 12pt;
  text-align: left;
  /* white-space: nowrap; */
  /* font-family: 'italic'; */
  /* overflow: hidden; */
  /* -o-text-overflow: ellipsis;
  text-overflow: ellipsis; */
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}



</style>
