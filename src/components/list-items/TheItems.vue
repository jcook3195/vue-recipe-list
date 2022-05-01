<template>
    <h1>{{ recipeName }}</h1>
    <base-card>
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
                recipeName: null
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
                // set the recipe name and get the components from the response to get the ingredients easier during the loop
                this.recipeName = response.data.name;
                const comps = response.data.sections[0].components;
                
                // set a counter for the random image generation
                let counter = 0;
                
                // loop through to get the ingredients & quantity
                comps.forEach((element) => {
                    const ingredient = element.ingredient.name;
                    const quantity = element.measurements[0].quantity;
                    counter++;

                    // add the new items to newItems 
                    const newItem = {
                        id: new Date().toISOString(),
                        title: ingredient,
                        quantity: quantity,
                        src: 'https://picsum.photos/200?random=' + counter
                    };

                    // add the new item to the recipe items
                    this.recipeItems.unshift(newItem);
                }); 
            }).catch(function (error) {
                console.error(error);
            }); 
        }
    }
</script>

<style scoped>
    h1 {
        text-align: center;
    }
</style>