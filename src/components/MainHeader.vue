<template>
    <header class="d-flex justify-content-between align-items-center">
        <nav>
            <h1>BOOLFLIX</h1>
        </nav>
        <BaseSearchInput @search="emitSearchTerm" placeholder="Cerca una produzione..." />
    </header>
</template>

<script>
import BaseSearchInput from './BaseSearchInput.vue';

export default {
    name: "MainHeader",
    methods: {
        emitSearchTerm(value) {
            this.$emit("search", value);
        },
    },
    components: { BaseSearchInput }
}
</script>

<style lang="scss" scoped>
header {
    background-color: #000;
    color: #fff;
    padding: 0px 20px;
}
</style>