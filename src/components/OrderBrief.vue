<template>
    <div class="order__body">
        <span class="name">
            {{id}}
        </span>
        <span class="cost">
            {{cost}} руб.
        </span>
    </div>
</template>

<script>
    export default {
        props: ['id', 'cost'],
    }
</script>

<style lang="scss" scoped>
    .order__body {
        width: 500px;
        height: 40px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        border: 3px solid black;
        padding: 0 1em 0 1em;
    }
</style>