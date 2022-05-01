<template>
    <the-header title="Vue Recipe Grocery List"></the-header>
    <div class="content-container">
        <the-items></the-items>
    </div>    
</template>

<script>
    import TheHeader from './components/layouts/TheHeader.vue';
    import TheItems from './components/list-items/TheItems.vue';

    export default {
        components: {
            TheHeader,
            TheItems
        }
    }
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

    * {
        box-sizing: border-box;
    }

    html {
        font-family: 'Roboto', sans-serif;
    }

    body {
        margin: 0;
    }

    @media only screen and (max-width: 760px) {
        .content-container {
            padding: 0 1rem;
        }
    }    
</style>
