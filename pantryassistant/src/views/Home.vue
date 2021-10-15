<template>
  <div class="home">
    <h1>Pantry Assistant</h1>
    <h2>What do you need to use up?</h2>
    <p>Note: this currently returns static data to save my API quotas</p>
    <input type="text" v-model="value" @change="getRecipes(value)"/>
    <!-- {{state.recipes}} -->
    <article v-for="recipe in state.recipes" :key="recipe">
      <!-- {{recipe}} -->
      <h3>{{ recipe.title }}</h3>
      <img :src="recipe.image">
      <h4>This will use up {{recipe.usedIngredientCount}} ingredients:</h4>
      <ul>
        <li v-for="usedIngredient in recipe.usedIngredients" :key="usedIngredient.id">
          {{ usedIngredient.name }}
        </li>
      </ul>
      
      <h4>You need to buy {{recipe.missedIngredientCount}} ingredients:</h4>
      <ul>
        <li v-for="missingIngredient in recipe.missedIngredients" :key="missingIngredient.id">
          {{ missingIngredient.name }}
        </li>
      </ul>

      <router-link :to="{ path: '/recipe/'+ recipe.id }">Link</router-link>
    </article>
  </div>
</template>

<script>

// VUE Composition API
import { reactive } from 'vue'
// @ is an alias to /src

export default {
  name: "Home",
  components: {
  },
  setup() {

    const state = reactive({
      api_key: "4afcbdd449da4a828e5ff1d2321c6eb8",
      url_base: "https://api.spoonacular.com",
      query: "",
      recipes: [],
      testData: [ { "id": 640352, "title": "Cranberry Apple Crisp", "image": "https://spoonacular.com/recipeImages/640352-312x231.jpg", "imageType": "jpg", "usedIngredientCount": 1, "missedIngredientCount": 3, "missedIngredients": [ { "id": 9078, "amount": 2, "unit": "cups", "unitLong": "cups", "unitShort": "cup", "aisle": "Produce", "name": "cranberries", "original": "2 cups fresh cranberries", "originalString": "2 cups fresh cranberries", "originalName": "fresh cranberries", "metaInformation": [ "fresh" ], "meta": [ "fresh" ], "extendedName": "fresh cranberries", "image": "https://spoonacular.com/cdn/ingredients_100x100/cranberries.jpg" }, { "id": 1145, "amount": 4, "unit": "Tbs", "unitLong": "Tbs", "unitShort": "Tbs", "aisle": "Milk, Eggs, Other Dairy", "name": "unsalted butter", "original": "1/2 stick (4 Tbs) unsalted butter, cut into cubes", "originalString": "1/2 stick (4 Tbs) unsalted butter, cut into cubes", "originalName": "1/2 stick unsalted butter, cut into cubes", "metaInformation": [ "unsalted", "cut into cubes" ], "meta": [ "unsalted", "cut into cubes" ], "image": "https://spoonacular.com/cdn/ingredients_100x100/butter-sliced.jpg" }, { "id": 8120, "amount": 1.5, "unit": "cups", "unitLong": "cups", "unitShort": "cup", "aisle": "Cereal", "name": "oats", "original": "1 1/2 cups regular oats (not quick-cooking)", "originalString": "1 1/2 cups regular oats (not quick-cooking)", "originalName": "regular oats (not quick-cooking)", "metaInformation": [ "(not quick-cooking)" ], "meta": [ "(not quick-cooking)" ], "image": "https://spoonacular.com/cdn/ingredients_100x100/rolled-oats.jpg" } ], "usedIngredients": [ { "id": 1089003, "amount": 4, "unit": "cups", "unitLong": "cups", "unitShort": "cup", "aisle": "Produce", "name": "granny smith apples", "original": "4 cups Granny Smith apples, chopped into ½ inch chunks", "originalString": "4 cups Granny Smith apples, chopped into ½ inch chunks", "originalName": "Granny Smith apples, chopped into ½ inch chunks", "metaInformation": [ "chopped" ], "meta": [ "chopped" ], "image": "https://spoonacular.com/cdn/ingredients_100x100/grannysmith-apple.png" } ], "unusedIngredients": [], "likes": 11 }, { "id": 641803, "title": "Easy & Delish! ~ Apple Crumble", "image": "https://spoonacular.com/recipeImages/641803-312x231.jpg", "imageType": "jpg", "usedIngredientCount": 1, "missedIngredientCount": 3, "missedIngredients": [ { "id": 1001, "amount": 0.75, "unit": "stick", "unitLong": "sticks", "unitShort": "stick", "aisle": "Milk, Eggs, Other Dairy", "name": "butter", "original": "3/4 stick of butter", "originalString": "3/4 stick of butter", "originalName": "butter", "metaInformation": [], "meta": [], "image": "https://spoonacular.com/cdn/ingredients_100x100/butter-sliced.jpg" }, { "id": 2011, "amount": 1, "unit": "Dash", "unitLong": "Dash", "unitShort": "Dash", "aisle": "Spices and Seasonings", "name": "cloves ground", "original": "Dash of ground cloves", "originalString": "Dash of ground cloves", "originalName": "ground cloves", "metaInformation": [], "meta": [], "image": "https://spoonacular.com/cdn/ingredients_100x100/cloves.jpg" }, { "id": 9156, "amount": 1, "unit": "", "unitLong": "", "unitShort": "", "aisle": "Produce", "name": "lemon zest", "original": "1 Zest of lemon", "originalString": "1 Zest of lemon", "originalName": "Zest of lemon", "metaInformation": [], "meta": [], "image": "https://spoonacular.com/cdn/ingredients_100x100/zest-lemon.jpg" } ], "usedIngredients": [ { "id": 9003, "amount": 3, "unit": "", "unitLong": "", "unitShort": "", "aisle": "Produce", "name": "apples", "original": "3 apples – sliced", "originalString": "3 apples – sliced", "originalName": "apples – sliced", "metaInformation": [ "sliced" ], "meta": [ "sliced" ], "image": "https://spoonacular.com/cdn/ingredients_100x100/apple.jpg" } ], "unusedIngredients": [], "likes": 1 } ]
    })

    function getRecipes(query) {
      state.query = query;
      console.log(state.query);
        // Simple GET request using fetch
      // fetch("https://api.spoonacular.com/recipes/findByIngredients?ingredients=apples,+flour,+sugar&number=2&apiKey="+state.api_key)
      //   .then(response => response.json())
      //   .then(data => (state.recipes = data));

      // Return dummy data
      state.recipes = state.testData;
    }


    return {
      state,
      getRecipes
    }
  }
};
</script>
