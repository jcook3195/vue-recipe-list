<template>
    <li>
        <base-card class="list-card">
            <div class="img-container">
                <img :src="src" :alt="title">
            </div>
            <div class="header">
                <h2>{{ title }} <span class="quantity">({{ quantity }})</span></h2>                
            </div>
            <base-button mode="flat" @click="deleteItem(id)">Delete</base-button>
            <div class="clearfix"></div>        
        </base-card>        
    </li>
</template>

<script>
    //import axios from 'axios';

    export default {
        props: ['id', 'title', 'quantity', 'src'],
        inject: ['deleteItem'],
    }
</script>

<style scoped>
    li {
        margin: auto;
        max-width: 40rem;
    }

    .list-card {
        display: flex;
        justify-content: space-between;
    }

    .header {
        min-width: 24rem;
        padding: 0 1rem;
    }

    h3 {
        font-size: 1.25rem;
    }

    .quantity {
        font-size: 1rem;
        font-weight: 400;
    }

    p {
        margin: 0.5rem 0;
    }

    a {
        text-decoration: none;
        color: #EF476F;
    }

    a:hover,
    a:active {
        color: #FFD166;
    }

    img {
        max-width: 70px;
        border-radius: 15px;
    }

    .img-container {
        float: left;
    }

    .clearfix {
        clear: both;
    }

    @media only screen and (max-width: 760px) {
        .header {
            min-width: none;
        }
    }
</style>
