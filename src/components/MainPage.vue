<template>
    <main>
        <section id="films" class="container">
            <h2>Film</h2>
            <div class="row g-1">
                <div v-for="film in filmsList" :key="film.id" class="col-3">
                    <SearchCard :production="film" />
                </div>
            </div>
        </section>

        <section id="tv-shows" class="container">
            <h2>Serie TV</h2>
            <div class="row g-1">
                <div v-for="show in showsList" :key="show.id" class="col-3">
                    <SearchCard :production="show" />
                </div>
            </div>
        </section>
    </main>
</template>

<script>
import SearchCard from './SearchCard.vue';
export default {
    name: "MainPage",
    data() {
        return {
            baseImgUri: "https://image.tmdb.org/t/p",
        };
    },
    props: {
        filmsList: Array,
        showsList: Array,
    },
    methods: {
        getCorrectVote(number) {
            return Math.ceil(number * 0.5);
        },
    },
    components: { SearchCard }
}
</script>

<style lang="scss" scoped>
main {
    background-color: #181818;
    color: #fff;

    section {
        margin-bottom: 100px;
    }
}
</style>