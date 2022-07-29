<template>
    <main>
        <section id="films" class="container">
            <h2>Film</h2>
            <div class="row">
                <div v-for="film in filmsList" :key="film.id" class="col">
                    <div class="_card">
                        <h4>{{ film.title }}</h4>
                        <div>{{ film.original_title }}</div>
                        <div v-if="film.poster_path">
                            <img :src="`${baseImgUri}/w342${film.poster_path}`" :alt="film.title">
                        </div>
                        <div v-if="film.original_language === 'it' || film.original_language === 'en'">
                            <img :src="require(`..//assets/img/${film.original_language}.png`)" alt="">
                        </div>
                        <div v-else>{{ film.original_language }}</div>
                        <div v-if="film.vote_average">
                            <i v-for="index in getCorrectVote(film.vote_average)" class="fa-solid fa-star"
                                :key="index"></i>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="tv-shows" class="container">
            <h2>Serie TV</h2>
            <div class="row">
                <div v-for="show in showsList" :key="show.id" class="col">
                    <div class="_card">
                        <h4>{{ show.name }}</h4>
                        <div>{{ show.original_name }}</div>
                        <div v-if="show.poster_path">
                            <img :src="`${baseImgUri}/w342${show.poster_path}`" :alt="show.name">
                        </div>
                        <div v-if="show.original_language === 'it' || show.original_language === 'en'">
                            <img :src="require(`../assets/img/${show.original_language}.png`)" alt="">
                        </div>
                        <div v-else>{{ show.original_language }}</div>
                        <div v-if="show.vote_average">
                            <i v-for="index in getCorrectVote(show.vote_average)" class="fa-solid fa-star"
                                :key="index"></i>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>

<script>
export default {
    name: 'MainPage',
    data() {
        return {
            baseImgUri: 'https://image.tmdb.org/t/p',
        }
    },
    props: {
        filmsList: Array,
        showsList: Array,
    },
    methods: {
        getCorrectVote(number) {
            return Math.ceil(number * 0.5);
        },
    }
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