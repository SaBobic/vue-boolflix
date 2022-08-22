<template>
    <main>
        <section id="search-term" class="container">
            <h2>Risultati per "{{ searchTerm }}".</h2>
        </section>

        <section v-if="filmsList.length > 0" id="films" class="container">
            <h3>Film</h3>
            <div class="row g-1">
                <div v-for="film in filmsList" :key="film.id" class="col-3">
                    <SearchCard :production="film" type="movie" base-img-uri="'https://image.tmdb.org/t/p'" />
                </div>
            </div>
        </section>

        <section v-if="showsList.length > 0" id="tv-shows" class="container">
            <h3>Serie TV</h3>
            <div class="row g-1">
                <div v-for="show in showsList" :key="show.id" class="col-3">
                    <SearchCard :production="show" type="tv" />
                </div>
            </div>
        </section>
    </main>
</template>

<script>
import SearchCard from './SearchCard.vue';
export default {
    name: "MainPage",
    props: {
        searchTerm: String,
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
    min-height: calc(100vh - 56px);
    background-color: #181818;
    padding: 30px 0;
    color: #fff;

    section {
        margin-bottom: 100px;
    }

    #search-term {
        padding: 0 10px;
        color: #aeaeae;
        margin-bottom: 30px;
        border-bottom: 1px solid #aeaeae;

        h2 {
            font-size: 20px;
        }
    }

}
</style>