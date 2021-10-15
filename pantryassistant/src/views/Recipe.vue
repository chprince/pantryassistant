<template>
  <div class="post layout-container">
   <img :src="state.recipe.image">
   <h1>{{state.recipe.title}}</h1>
   <div class="recipe-meta">
       Serves {{state.recipe.servings}} | {{state.recipe.readyInMinutes}} minutes
   </div>
   <p>By <a :href="state.recipe.sourceUrl">{{state.recipe.sourceName}}</a></p>
   <h2>Dietary Stuff</h2>
   
   <ul>
       <li>Dairy Free: {{state.recipe.dairyFree}}</li>
       <li>Gluten Free: {{state.recipe.glutenFree}}</li>
       <li>Vegan: {{state.recipe.vegan}}</li>
       <li>Vegetarian: {{state.recipe.vegetarian}}</li>
       <li v-for="diet in state.recipe.diets" :key="diet">{{diet}}</li>
   </ul>

   <h2>Tags</h2>
   <ul>
      <li v-for="tag in state.recipe.dishTypes" :key="tag">{{tag}}</li>
   </ul>

   <h2>Ingredients</h2>
   <!-- {{state.recipe.extendedIngredients}} -->
   <form @change="setUnits(units)">
        <input type="radio" id="metric" name="unitType" value="metric" v-model="units" checked>
        <label for="metric">Metric</label><br>
        <input type="radio" id="us" name="unitType" value="us" v-model="units">
        <label for="us">US</label><br>
   </form>
   <ol>
       <li  v-for="item in state.recipe.extendedIngredients" :key="item">
            <span v-if="state.units === 'metric'">
               {{item.measures.metric.amount}}{{item.measures.metric.unitShort}}
            </span>
            <span v-else>
               {{item.measures.us.amount}}{{item.measures.us.unitShort}}
            </span>
            {{item.originalName}}
       </li>
   </ol>
   <button @click="getInstructions($route.params.id)">Start Cooking</button>
   <!-- {{state.instructions}} -->
   <ol>
       <li v-for="step in state.instructions.steps" :key="step.number">
           {{step.step}}
           
           <h4>Ingredients needed</h4>
           <ul>
               <li v-for="ingredient in step.ingredients" :key="ingredient">
                   {{ingredient.name}}
               </li>
           </ul>

            <h4>Equipment needed</h4>
           <ul>
               <li v-for="equipment in step.equipment" :key="equipment">
                   {{equipment.name}}
               </li>
           </ul>
       </li>
   </ol>
  </div>
</template>

<script>
import { reactive } from 'vue';
import { useRoute } from 'vue-router'

export default {
  name: "Recipe",
    setup() {
        const route = useRoute()

        const state = reactive({
        api_key: "4afcbdd449da4a828e5ff1d2321c6eb8",
        url_base: "https://api.spoonacular.com",
        query: "",
        recipe: {},
        instructions: {},
        units: 'metric',
        testData: {
    "vegetarian": true,
    "vegan": false,
    "glutenFree": false,
    "dairyFree": false,
    "veryHealthy": false,
    "cheap": false,
    "veryPopular": false,
    "sustainable": false,
    "weightWatcherSmartPoints": 37,
    "gaps": "no",
    "lowFodmap": false,
    "aggregateLikes": 11,
    "spoonacularScore": 35.0,
    "healthScore": 4.0,
    "creditsText": "Foodista.com – The Cooking Encyclopedia Everyone Can Edit",
    "license": "CC BY 3.0",
    "sourceName": "Foodista",
    "pricePerServing": 153.59,
    "extendedIngredients": [
        {
            "id": 1089003,
            "aisle": "Produce",
            "image": "grannysmith-apple.png",
            "consistency": "solid",
            "name": "granny smith apples",
            "nameClean": "granny smith apple",
            "original": "4 cups Granny Smith apples, chopped into ½ inch chunks",
            "originalString": "4 cups Granny Smith apples, chopped into ½ inch chunks",
            "originalName": "Granny Smith apples, chopped into ½ inch chunks",
            "amount": 4.0,
            "unit": "cups",
            "meta": [
                "chopped"
            ],
            "metaInformation": [
                "chopped"
            ],
            "measures": {
                "us": {
                    "amount": 4.0,
                    "unitShort": "cups",
                    "unitLong": "cups"
                },
                "metric": {
                    "amount": 946.352,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 9078,
            "aisle": "Produce",
            "image": "cranberries.jpg",
            "consistency": "solid",
            "name": "cranberries",
            "nameClean": "cranberries",
            "original": "2 cups fresh cranberries",
            "originalString": "2 cups fresh cranberries",
            "originalName": "fresh cranberries",
            "amount": 2.0,
            "unit": "cups",
            "meta": [
                "fresh"
            ],
            "metaInformation": [
                "fresh"
            ],
            "measures": {
                "us": {
                    "amount": 2.0,
                    "unitShort": "cups",
                    "unitLong": "cups"
                },
                "metric": {
                    "amount": 473.176,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 19335,
            "aisle": "Baking",
            "image": "sugar-in-bowl.png",
            "consistency": "solid",
            "name": "sugar",
            "nameClean": "sugar",
            "original": "1 cup sugar",
            "originalString": "1 cup sugar",
            "originalName": "sugar",
            "amount": 1.0,
            "unit": "cup",
            "meta": [],
            "metaInformation": [],
            "measures": {
                "us": {
                    "amount": 1.0,
                    "unitShort": "cup",
                    "unitLong": "cup"
                },
                "metric": {
                    "amount": 236.588,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 1145,
            "aisle": "Milk, Eggs, Other Dairy",
            "image": "butter-sliced.jpg",
            "consistency": "solid",
            "name": "unsalted butter",
            "nameClean": "unsalted butter",
            "original": "1/2 stick (4 Tbs) unsalted butter, cut into cubes",
            "originalString": "1/2 stick (4 Tbs) unsalted butter, cut into cubes",
            "originalName": "1/2 stick unsalted butter, cut into cubes",
            "amount": 4.0,
            "unit": "Tbs",
            "meta": [
                "unsalted",
                "cut into cubes"
            ],
            "metaInformation": [
                "unsalted",
                "cut into cubes"
            ],
            "measures": {
                "us": {
                    "amount": 4.0,
                    "unitShort": "Tbs",
                    "unitLong": "Tbs"
                },
                "metric": {
                    "amount": 4.0,
                    "unitShort": "Tbs",
                    "unitLong": "Tbs"
                }
            }
        },
        {
            "id": 8120,
            "aisle": "Cereal",
            "image": "rolled-oats.jpg",
            "consistency": "solid",
            "name": "oats",
            "nameClean": "rolled oats",
            "original": "1 1/2 cups regular oats (not quick-cooking)",
            "originalString": "1 1/2 cups regular oats (not quick-cooking)",
            "originalName": "regular oats (not quick-cooking)",
            "amount": 1.5,
            "unit": "cups",
            "meta": [
                "(not quick-cooking)"
            ],
            "metaInformation": [
                "(not quick-cooking)"
            ],
            "measures": {
                "us": {
                    "amount": 1.5,
                    "unitShort": "cups",
                    "unitLong": "cups"
                },
                "metric": {
                    "amount": 354.882,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 19334,
            "aisle": "Baking",
            "image": "light-brown-sugar.jpg",
            "consistency": "solid",
            "name": "light brown sugar",
            "nameClean": "golden brown sugar",
            "original": "1/2 cup light brown sugar",
            "originalString": "1/2 cup light brown sugar",
            "originalName": "light brown sugar",
            "amount": 0.5,
            "unit": "cup",
            "meta": [
                "light"
            ],
            "metaInformation": [
                "light"
            ],
            "measures": {
                "us": {
                    "amount": 0.5,
                    "unitShort": "cups",
                    "unitLong": "cups"
                },
                "metric": {
                    "amount": 118.294,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 20081,
            "aisle": "Baking",
            "image": "flour.png",
            "consistency": "solid",
            "name": "flour",
            "nameClean": "wheat flour",
            "original": "1/4 cup all-purpose flour",
            "originalString": "1/4 cup all-purpose flour",
            "originalName": "all-purpose flour",
            "amount": 0.25,
            "unit": "cup",
            "meta": [
                "all-purpose"
            ],
            "metaInformation": [
                "all-purpose"
            ],
            "measures": {
                "us": {
                    "amount": 0.25,
                    "unitShort": "cups",
                    "unitLong": "cups"
                },
                "metric": {
                    "amount": 59.147,
                    "unitShort": "ml",
                    "unitLong": "milliliters"
                }
            }
        },
        {
            "id": 1145,
            "aisle": "Milk, Eggs, Other Dairy",
            "image": "butter-sliced.jpg",
            "consistency": "solid",
            "name": "unsalted butter",
            "nameClean": "unsalted butter",
            "original": "1 stick unsalted butter, melted",
            "originalString": "1 stick unsalted butter, melted",
            "originalName": "unsalted butter, melted",
            "amount": 1.0,
            "unit": "stick",
            "meta": [
                "unsalted",
                "melted"
            ],
            "metaInformation": [
                "unsalted",
                "melted"
            ],
            "measures": {
                "us": {
                    "amount": 1.0,
                    "unitShort": "stick",
                    "unitLong": "stick"
                },
                "metric": {
                    "amount": 1.0,
                    "unitShort": "stick",
                    "unitLong": "stick"
                }
            }
        }
    ],
    "id": 640352,
    "title": "Cranberry Apple Crisp",
    "readyInMinutes": 45,
    "servings": 4,
    "sourceUrl": "https://www.foodista.com/recipe/TJXDHDFD/cranberry-apple-crisp",
    "image": "https://spoonacular.com/recipeImages/640352-556x370.jpg",
    "imageType": "jpg",
    "summary": "If you want to add more <b>lacto ovo vegetarian</b> recipes to your recipe box, Cranberry Apple Crisp might be a recipe you should try. This recipe serves 4 and costs $1.54 per serving. One serving contains <b>834 calories</b>, <b>6g of protein</b>, and <b>37g of fat</b>. It works best as a dessert, and is done in roughly <b>roughly 45 minutes</b>. It is brought to you by Foodista. 11 person found this recipe to be scrumptious and satisfying. If you have granny smith apples, cranberries, butter, and a few other ingredients on hand, you can make it. With a spoonacular <b>score of 33%</b>, this dish is rather bad. If you like this recipe, you might also like recipes such as <a href=\"https://spoonacular.com/recipes/cranberry-apple-crisp-1050678\">Cranberry Apple Crisp</a>, <a href=\"https://spoonacular.com/recipes/cranberry-apple-crisp-48113\">Cranberry Apple Crisp</a>, and <a href=\"https://spoonacular.com/recipes/apple-cranberry-crisp-597825\">Apple Cranberry Crisp</a>.",
    "cuisines": [],
    "dishTypes": [
        "dessert"
    ],
    "diets": [
        "lacto ovo vegetarian"
    ],
    "occasions": [],
    "winePairing": {
        "pairedWines": [
            "cream sherry",
            "moscato dasti",
            "port"
        ],
        "pairingText": "Cream Sherry, Moscato d'Asti, and Port are my top picks for Crisp. A common wine pairing rule is to make sure your wine is sweeter than your food. Delicate desserts go well with Moscato d'Asti, nutty desserts with cream sherry, and caramel or chocolate desserts pair well with port. You could try NV Solera Cream Sherry. Reviewers quite like it with a 4.5 out of 5 star rating and a price of about 17 dollars per bottle.",
        "productMatches": [
            {
                "id": 428475,
                "title": "NV Solera Cream Sherry",
                "description": "The Solera Cream Sherry has a brilliant amber and deep copper hue. With butterscotch and pecan aromas, the sweet salted nut and brown spice aromas carry a complex caramel accent. A sweet entry leads to a rounded, lush, moderately full-bodied palate with a lengthy, flavorful finish.",
                "price": "$16.99",
                "imageUrl": "https://spoonacular.com/productImages/428475-312x231.jpg",
                "averageRating": 0.9,
                "ratingCount": 4.0,
                "score": 0.823076923076923,
                "link": "https://www.amazon.com/NV-Solera-Cream-Sherry-750/dp/B00HSME8OW?tag=spoonacular-20"
            }
        ]
    },
    "instructions": "Preheat the oven to 350 degrees and grease or butter a 913 glass baking dish.\nIn a large bowl, toss together the chopped apples, cranberries and sugar. Let stand for a few minutes then pour into the baking dish.\nDot the mixture with the 1/2 stick of cubed butter.\nIn a medium bowl, combine the oats, brown sugar and flour.  Sprinkle evenly over the cranberries and apples in the baking dish.  Gently pour the melted butter over the top.\nCover with aluminum foil and bake for 35 minutes.  Remove the foil and bake for an additional 15 minutes, or until the oat topping is nicely browned.\nServe warm as a side or for dessert with a scoop of vanilla ice cream.",
    "analyzedInstructions": [
        {
            "name": "",
            "steps": [
                {
                    "number": 1,
                    "step": "Preheat the oven to 350 degrees and grease or butter a 913 glass baking dish.",
                    "ingredients": [
                        {
                            "id": 1001,
                            "name": "butter",
                            "localizedName": "butter",
                            "image": "butter-sliced.jpg"
                        }
                    ],
                    "equipment": [
                        {
                            "id": 406921,
                            "name": "glass baking pan",
                            "localizedName": "glass baking pan",
                            "image": "glass-baking-tray.jpg"
                        },
                        {
                            "id": 404784,
                            "name": "oven",
                            "localizedName": "oven",
                            "image": "oven.jpg"
                        }
                    ]
                },
                {
                    "number": 2,
                    "step": "In a large bowl, toss together the chopped apples, cranberries and sugar.",
                    "ingredients": [
                        {
                            "id": 9078,
                            "name": "cranberries",
                            "localizedName": "cranberries",
                            "image": "cranberries.jpg"
                        },
                        {
                            "id": 9003,
                            "name": "apple",
                            "localizedName": "apple",
                            "image": "apple.jpg"
                        },
                        {
                            "id": 19335,
                            "name": "sugar",
                            "localizedName": "sugar",
                            "image": "sugar-in-bowl.png"
                        }
                    ],
                    "equipment": [
                        {
                            "id": 404783,
                            "name": "bowl",
                            "localizedName": "bowl",
                            "image": "bowl.jpg"
                        }
                    ]
                },
                {
                    "number": 3,
                    "step": "Let stand for a few minutes then pour into the baking dish.",
                    "ingredients": [],
                    "equipment": [
                        {
                            "id": 404646,
                            "name": "baking pan",
                            "localizedName": "baking pan",
                            "image": "roasting-pan.jpg"
                        }
                    ]
                },
                {
                    "number": 4,
                    "step": "Dot the mixture with the 1/2 stick of cubed butter.",
                    "ingredients": [
                        {
                            "id": 1001,
                            "name": "butter",
                            "localizedName": "butter",
                            "image": "butter-sliced.jpg"
                        }
                    ],
                    "equipment": []
                },
                {
                    "number": 5,
                    "step": "In a medium bowl, combine the oats, brown sugar and flour.",
                    "ingredients": [
                        {
                            "id": 19334,
                            "name": "brown sugar",
                            "localizedName": "brown sugar",
                            "image": "dark-brown-sugar.png"
                        },
                        {
                            "id": 20081,
                            "name": "all purpose flour",
                            "localizedName": "all purpose flour",
                            "image": "flour.png"
                        },
                        {
                            "id": 8120,
                            "name": "oats",
                            "localizedName": "oats",
                            "image": "rolled-oats.jpg"
                        }
                    ],
                    "equipment": [
                        {
                            "id": 404783,
                            "name": "bowl",
                            "localizedName": "bowl",
                            "image": "bowl.jpg"
                        }
                    ]
                },
                {
                    "number": 6,
                    "step": "Sprinkle evenly over the cranberries and apples in the baking dish.  Gently pour the melted butter over the top.",
                    "ingredients": [
                        {
                            "id": 9078,
                            "name": "cranberries",
                            "localizedName": "cranberries",
                            "image": "cranberries.jpg"
                        },
                        {
                            "id": 9003,
                            "name": "apple",
                            "localizedName": "apple",
                            "image": "apple.jpg"
                        },
                        {
                            "id": 1001,
                            "name": "butter",
                            "localizedName": "butter",
                            "image": "butter-sliced.jpg"
                        }
                    ],
                    "equipment": [
                        {
                            "id": 404646,
                            "name": "baking pan",
                            "localizedName": "baking pan",
                            "image": "roasting-pan.jpg"
                        }
                    ]
                },
                {
                    "number": 7,
                    "step": "Cover with aluminum foil and bake for 35 minutes.",
                    "ingredients": [],
                    "equipment": [
                        {
                            "id": 404765,
                            "name": "aluminum foil",
                            "localizedName": "aluminum foil",
                            "image": "aluminum-foil.png"
                        },
                        {
                            "id": 404784,
                            "name": "oven",
                            "localizedName": "oven",
                            "image": "oven.jpg"
                        }
                    ],
                    "length": {
                        "number": 35,
                        "unit": "minutes"
                    }
                },
                {
                    "number": 8,
                    "step": "Remove the foil and bake for an additional 15 minutes, or until the oat topping is nicely browned.",
                    "ingredients": [],
                    "equipment": [
                        {
                            "id": 404784,
                            "name": "oven",
                            "localizedName": "oven",
                            "image": "oven.jpg"
                        },
                        {
                            "id": 404765,
                            "name": "aluminum foil",
                            "localizedName": "aluminum foil",
                            "image": "aluminum-foil.png"
                        }
                    ],
                    "length": {
                        "number": 15,
                        "unit": "minutes"
                    }
                },
                {
                    "number": 9,
                    "step": "Serve warm as a side or for dessert with a scoop of vanilla ice cream.",
                    "ingredients": [
                        {
                            "id": 19095,
                            "name": "vanilla ice cream",
                            "localizedName": "vanilla ice cream",
                            "image": "vanilla-ice-cream.png"
                        }
                    ],
                    "equipment": []
                }
            ]
        }
    ],
    "originalId": null,
    "spoonacularSourceUrl": "https://spoonacular.com/cranberry-apple-crisp-640352"
},
        testInstructions:  [
  {
    "name": "",
    "steps": [
      {
        "number": 1,
        "step": "Preheat the oven to 350 degrees and grease or butter a 913 glass baking dish.",
        "ingredients": [
          {
            "id": 1001,
            "name": "butter",
            "localizedName": "butter",
            "image": "butter-sliced.jpg"
          }
        ],
        "equipment": [
          {
            "id": 406921,
            "name": "glass baking pan",
            "localizedName": "glass baking pan",
            "image": "glass-baking-tray.jpg"
          },
          {
            "id": 404784,
            "name": "oven",
            "localizedName": "oven",
            "image": "oven.jpg"
          }
        ]
      },
      {
        "number": 2,
        "step": "In a large bowl, toss together the chopped apples, cranberries and sugar.",
        "ingredients": [
          {
            "id": 9078,
            "name": "cranberries",
            "localizedName": "cranberries",
            "image": "cranberries.jpg"
          },
          {
            "id": 9003,
            "name": "apple",
            "localizedName": "apple",
            "image": "apple.jpg"
          },
          {
            "id": 19335,
            "name": "sugar",
            "localizedName": "sugar",
            "image": "sugar-in-bowl.png"
          }
        ],
        "equipment": [
          {
            "id": 404783,
            "name": "bowl",
            "localizedName": "bowl",
            "image": "bowl.jpg"
          }
        ]
      },
      {
        "number": 3,
        "step": "Let stand for a few minutes then pour into the baking dish.",
        "ingredients": [],
        "equipment": [
          {
            "id": 404646,
            "name": "baking pan",
            "localizedName": "baking pan",
            "image": "roasting-pan.jpg"
          }
        ]
      },
      {
        "number": 4,
        "step": "Dot the mixture with the 1/2 stick of cubed butter.",
        "ingredients": [
          {
            "id": 1001,
            "name": "butter",
            "localizedName": "butter",
            "image": "butter-sliced.jpg"
          }
        ],
        "equipment": []
      },
      {
        "number": 5,
        "step": "In a medium bowl, combine the oats, brown sugar and flour.",
        "ingredients": [
          {
            "id": 19334,
            "name": "brown sugar",
            "localizedName": "brown sugar",
            "image": "dark-brown-sugar.png"
          },
          {
            "id": 20081,
            "name": "all purpose flour",
            "localizedName": "all purpose flour",
            "image": "flour.png"
          },
          {
            "id": 8120,
            "name": "oats",
            "localizedName": "oats",
            "image": "rolled-oats.jpg"
          }
        ],
        "equipment": [
          {
            "id": 404783,
            "name": "bowl",
            "localizedName": "bowl",
            "image": "bowl.jpg"
          }
        ]
      },
      {
        "number": 6,
        "step": "Sprinkle evenly over the cranberries and apples in the baking dish.  Gently pour the melted butter over the top.",
        "ingredients": [
          {
            "id": 9078,
            "name": "cranberries",
            "localizedName": "cranberries",
            "image": "cranberries.jpg"
          },
          {
            "id": 9003,
            "name": "apple",
            "localizedName": "apple",
            "image": "apple.jpg"
          },
          {
            "id": 1001,
            "name": "butter",
            "localizedName": "butter",
            "image": "butter-sliced.jpg"
          }
        ],
        "equipment": [
          {
            "id": 404646,
            "name": "baking pan",
            "localizedName": "baking pan",
            "image": "roasting-pan.jpg"
          }
        ]
      },
      {
        "number": 7,
        "step": "Cover with aluminum foil and bake for 35 minutes.",
        "ingredients": [],
        "equipment": [
          {
            "id": 404765,
            "name": "aluminum foil",
            "localizedName": "aluminum foil",
            "image": "aluminum-foil.png"
          },
          {
            "id": 404784,
            "name": "oven",
            "localizedName": "oven",
            "image": "oven.jpg"
          }
        ],
        "length": {
          "number": 35,
          "unit": "minutes"
        }
      },
      {
        "number": 8,
        "step": "Remove the foil and bake for an additional 15 minutes, or until the oat topping is nicely browned.",
        "ingredients": [],
        "equipment": [
          {
            "id": 404784,
            "name": "oven",
            "localizedName": "oven",
            "image": "oven.jpg"
          },
          {
            "id": 404765,
            "name": "aluminum foil",
            "localizedName": "aluminum foil",
            "image": "aluminum-foil.png"
          }
        ],
        "length": {
          "number": 15,
          "unit": "minutes"
        }
      },
      {
        "number": 9,
        "step": "Serve warm as a side or for dessert with a scoop of vanilla ice cream.",
        "ingredients": [
          {
            "id": 19095,
            "name": "vanilla ice cream",
            "localizedName": "vanilla ice cream",
            "image": "vanilla-ice-cream.png"
          }
        ],
        "equipment": []
      }
    ]
  }
] 
        })

        function getRecipe() {
            // url for instructions call: GET https://api.spoonacular.com/recipes/{id}/analyzedInstructions
            console.log(route);
            // Simple GET request using fetch
        // fetch("https://api.spoonacular.com/recipes/640352/information?apiKey=4afcbdd449da4a828e5ff1d2321c6eb8")
        //     .then(response => response.json())
        //     .then(data => (state.recipe = data));

        // Return dummy data
        state.recipe = state.testData;
        }

        getRecipe();

        function setUnits(unit) {
            state.units = unit;
        }

        setUnits(state.units);

        function getInstructions(id) {
            console.log(id);
            // fetch("https://api.spoonacular.com/recipes/" + id + "/analyzedInstructions?apiKey=4afcbdd449da4a828e5ff1d2321c6eb8")
            // .then(response => response.json())
            // .then(data => (state.instructions = data[0]));
            state.instructions = state.testInstructions[0];
        }

        return {
        state,
        getRecipe,
        setUnits,
        getInstructions
        }
    },
};
</script>

<style scoped lang="scss">

</style>
