<template>
    <div class="_card">
        <h4>{{ production.title || production.name }}</h4>
        <div>{{ production.original_title || production.original_name }}</div>
        <div v-if="production.poster_path">
            <img :src="`${baseImgUri}/w342${production.poster_path}`" :alt="production.title || production.name">
        </div>
        <div v-if="production.original_language === 'it' || production.original_language === 'en'">
            <img :src="require(`../assets/img/${production.original_language}.png`)" alt="">
        </div>
        <div v-else>{{ production.original_language }}</div>
        <div v-if="production.vote_average">
            <i v-for="index in getCorrectVote(production.vote_average)" class="fa-solid fa-star" :key="index"></i>
        </div>
    </div>
</template>

<script>
export default {
    name: 'BaseCard',
    data() {
        return {
            baseImgUri: "https://image.tmdb.org/t/p",
        };
    },
    props: {
        production: Object,
    },
    methods: {
        getCorrectVote(number) {
            return Math.ceil(number * 0.5);
        },
    }
}
</script>

<style lang="scss" scoped>
</style>