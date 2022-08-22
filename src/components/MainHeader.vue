<template>
    <header class="d-flex justify-content-between align-items-center">
        <nav>
            <h1>BOOLFLIX</h1>
        </nav>
        <div>
            <BaseSelect @select="emitSearchGenre" :options="genresList" />
            <BaseSearchInput @search="emitSearchTerm" placeholder="Cerca una produzione..." />
        </div>
    </header>
</template>

<script>
import BaseSearchInput from './BaseSearchInput.vue';
import BaseSelect from './BaseSelect.vue';

export default {
    name: "MainHeader",
    props: {
        genresList: Array,
    },
    methods: {
        emitSearchTerm(value) {
            this.$emit("search", value);
        },
        emitSearchGenre(value) {
            this.$emit("select", value);
        },
    },
    components: { BaseSearchInput, BaseSelect }
}
</script>

<style lang="scss" scoped>
header {
    background-color: #000;
    color: #fff;
    padding: 0px 20px;
}
</style>