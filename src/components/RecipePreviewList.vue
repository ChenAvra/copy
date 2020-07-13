<template>
  <b-container>
    <h3>{{ title }}:</h3>

    <div v-for="r in recipesMainPage" :key="r.id">
      <RecipePreview
        class="recipePreview"
        :recipe="r"
        :isLogin="isLoginPreList"
      />
    </div>

    <div v-if="this.title == 'Explore these recipes'">
      <ButtonRandom v-on:addItem="this.updateRecipes"> </ButtonRandom>
    </div>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
import ButtonRandom from "../components/ButtonRandom.vue";

export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
    ButtonRandom,
  },

  props: {
    title: {
      type: String,
      required: true,
    },
    isLoginPreList: {
      type: String,
      required: true,
    },
  },

  data() {
    return {
      recipesMainPage: [],
    };
  },

  created() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      let urlPath = "";
      if (this.title == "Explore these recipes") {
        urlPath = "http://localhost:3000/recipes/threeRandomRecipies";
      } else if (this.title == "Last watched recipes") {
        urlPath = "http://localhost:3000/users/threeLastRecipes";
      } else if (this.title == "Your favorite recipes") {
        urlPath = "http://localhost:3000/users/myFavorites";
      }
      try {
        const response = await this.axios.get(urlPath);
        this.recipesMainPage = [];

        const recipes = response.data;
        let tempArray = [];
        tempArray.push(...recipes);
        console.log("ksa");

        // console.log(this.recipesMainPage, "recipes Main page");

        if (this.$root.store.username) {
          let recipesiD = [];

          for (let i = 0; i < 3; i++) {
            if (tempArray[i]) {
              recipesiD.push(tempArray[i].id);
              console.log(tempArray[i].id, "rcipe id");
            }
          }

          const responseInfo = await this.axios.get(
            "http://localhost:3000/users/recipeInfo/[" + recipesiD + "]"
          );

          console.log(responseInfo, "info array");

          for (let i = 0; i < 3; i++) {
            if (tempArray[i]) {
              // if(responseInfo.data[this.recipesMainPage[i].id][1].saveFavorites)
              tempArray[i].isFavorite =
                responseInfo.data[tempArray[i].id][1].saveFavorites;
              console.log(
                responseInfo.data[tempArray[i].id][1].saveFavorites,
                "is favorires"
              );

              tempArray[i].isWatched =
                responseInfo.data[tempArray[i].id][0].watched;
              console.log(
                responseInfo.data[tempArray[i].id][0].watched,
                "is watched"
              );
            }
          }
        }
        this.recipesMainPage.push(...tempArray);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
