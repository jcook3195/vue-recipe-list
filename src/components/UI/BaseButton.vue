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
        background-color: #457B9D;
        border: none;
        color: white;
        cursor: pointer;
    }

    button:hover,
    button:active {
        background-color: #1D3557;
    }

    .flat {
        background-color: transparent;
        color: #457B9D;
    }

    .flat:hover,
    .flat:active {
        color: #fff;
    }
</style>