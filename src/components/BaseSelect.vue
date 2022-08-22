<template>
    <select name="" id="" v-model="selectedOption" @change="emitSelect()">
        <option value="" default>Seleziona un genere</option>
        <option v-for="option in options" :key="option.id" :value="option.id">{{ option.name }}</option>
    </select>
</template>

<script>
export default {
    name: 'BaseSelect',
    props: {
        options: Array,
    },
    data() {
        return {
            selectedOption: '',
        }
    },
    methods: {
        emitSelect() {
            this.$emit('select', this.selectedOption);
        }
    }
}
</script>

<style lang="scss" scoped>
select {
    margin-right: 15px;
}
</style>