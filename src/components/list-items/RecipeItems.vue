<template>
    <ul>
        <list-item v-for="item in items" :key="item.id" :id="item.id" :title="item.title" :quantity="item.quantity" :src="item.src"></list-item>
    </ul>
</template>

<script>
    import ListItem from './ListItem.vue';

    export default {
        inject: ['items'],
        components: {
            ListItem
        }
    }
</script>

<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
        margin: auto;
        max-width: 40rem;
    }
</style>