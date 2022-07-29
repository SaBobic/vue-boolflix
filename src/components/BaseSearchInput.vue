<template>
    <input type="text" :placeholder="placeholder || 'Search...'" v-model="term" @keyup.enter="emitSearch" />
</template>

<script>
export default {
    name: 'BaseSearchInput',
    props: {
        placeholder: String,
    },
    data() {
        return {
            term: "",
        }
    },
    methods: {
        emitSearch() {
            if (this.term) {
                this.$emit('search', this.term);
                this.term = "";
            }
        }
    }
}
</script>

<style lang="scss" scoped>
</style>