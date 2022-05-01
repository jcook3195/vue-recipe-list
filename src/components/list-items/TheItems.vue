<template>
    <base-modal v-if="recipeLoading" title="Getting Random Recipe Data" @closeModal="confirmError">
        <template #default>
            <p>Loading...</p>
        </template>
        <template #actions>
            &nbsp;            
        </template>
    </base-modal>
    <div class="img-container">
        <img :src="imgSrc" alt="Recipe Image">
    </div>
    <h1>{{ recipeName }}</h1>
    <div class="nutrition-facts">
        <div>Calories: <strong>{{ recipeCals }}</strong></div>
        <div>Protein: <strong>{{ recipeProtein }}g</strong></div>
        <div>Carbs: <strong>{{ recipeCarbs }}g</strong></div>
        <div>Fat: <strong>{{ recipeFat }}g</strong></div>
    </div>    
    <base-card class="button-container">
        <base-button @click="setSelectedTab('recipe-items')" :mode="recipeItemButtonMode">List</base-button>
        <base-button @click="setSelectedTab('add-item')" :mode="addItemButtonMode">Add Item</base-button>
        <p>{{ info }}</p>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
    import RecipeItems from './RecipeItems.vue'
    import AddItem from './AddItem.vue';

    export default {
        components: {
            RecipeItems,
            AddItem
        },
        data() {
            return {
                selectedTab: 'recipe-items',
                recipeItems: [],
                imgSrc: null,
                recipeName: null,
                recipeCals: null,
                recipeProtein: null,
                recipeCarbs: null,
                recipeFat: null,
                recipeLoading: true
            };
        },
        provide() {
            return {
                items: this.recipeItems,
                addItem: this.addItem,
                deleteItem: this.removeItem
            };
        },
        computed: {
            recipeItemButtonMode() {
                return this.selectedTab === 'recipe-items' ? null : 'flat';
            },
            addItemButtonMode() {
                return this.selectedTab === 'add-item' ? null : 'flat';
            }
        },
        methods: {
            setSelectedTab(tab) {
                this.selectedTab = tab;
            },
            addItem(title, quantity, url) {
                const newItem = {
                    id: new Date().toISOString(),
                    title: title,
                    quantity: quantity,
                    src: url
                };

                this.recipeItems.unshift(newItem);
                this.selectedTab = 'recipe-items';
            },
            removeItem(itemId) {
                const itemIndex = this.recipeItems.findIndex(item => item.id == itemId);
                this.recipeItems.splice(itemIndex, 1);
            },
            randomNumb(min, max) { // min and max included 
                return Math.floor(Math.random() * (max - min + 1) + min)
            }
        },
        mounted() {
            const axios = require("axios");

            // get a random ID to generate a random recipe every load
            const randomID = this.randomNumb(1, 1000);

            // set the axios params
            const options = {
                method: 'GET',
                url: 'https://tasty.p.rapidapi.com/recipes/get-more-info',
                params: {id: randomID},
                headers: {
                    'X-RapidAPI-Host': 'tasty.p.rapidapi.com',
                    'X-RapidAPI-Key': 'b8e3fe108fmsh81f9b4cccabb7a9p19581ejsn1732d85dad07'
                }
            };

            axios.request(options).then(response => {
                // get the needed information from the response
                this.recipeName = response.data.name;
                const comps = response.data.sections[0].components;
                const nutrition = response.data.nutrition;
                this.imgSrc = response.data.thumbnail_url;
                this.recipeCals = nutrition.calories;
                this.recipeProtein = nutrition.protein;
                this.recipeCarbs = nutrition.carbohydrates;
                this.recipeFat = nutrition.fat;
                
                // set a counter for the random image generation
                let counter = 0;
                
                // loop through to get the ingredients & quantity
                comps.forEach((element) => {
                    const ingredient = element.ingredient.name;
                    const quantity = element.measurements[0].quantity;
                    const unit = element.measurements[0].unit.display_plural;
                    counter++;

                    // add the new items to newItems 
                    const newItem = {
                        id: new Date().toISOString(),
                        title: ingredient,
                        quantity: quantity + ' ' + unit,
                        src: 'https://picsum.photos/200?random=' + counter
                    };

                    // add the new item to the recipe items
                    this.recipeItems.unshift(newItem);
                }); 

                this.recipeLoading = false;
            }).catch(error => {
                this.recipeLoading = false;
                console.error(error);
            }); 
        }
    }
</script>

<style scoped>
    h1 {
        text-align: center;
        margin-top: 0;
    }

    .nutrition-facts {
        display: flex;
        max-width: 40rem;
        margin: auto;
        align-items: center;
        justify-content: space-around;
        font-size: 1.25rem;
    }

    .img-container {
        max-width: 40rem;
        margin: auto;
        text-align: center;
    }

    img {
        width: 20rem;
        margin: 1rem auto;
        border: 8px solid #E63946;
        border-radius: 50%;
    }

    .button-container {
        display: flex;
        justify-content: center;
    }

</style>