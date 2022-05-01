<template>
    <button :type="type" :class="mode">
        <slot></slot>
    </button>
</template>

<script>
export default {
    props: ['type', 'mode']
}
</script>

<style scoped>
    button {
        padding: 0.75rem 1.5rem;
        font-family: inherit;
        background-color: #118AB2;
        border: none;
        color: white;
        cursor: pointer;
    }

    button:hover,
    button:active {
        background-color: #073B4C;
    }

    .flat {
        background-color: transparent;
        color: #118AB2;
    }

    .flat:hover,
    .flat:active {
        color: #fff;
    }
</style>