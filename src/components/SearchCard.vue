<template>
    <article>
        <figure @click="fetchDetails()">
            <img :src="getPoster()" :alt="production.title || production.name">
            <h4 v-if="!production.poster_path">{{ production.title || production.name }}</h4>
        </figure>
        <FullProd v-if="display" :production="production" :base-img-uri="baseImgUri" @display="hideFullProd"
            :seasons="seasons" :runtime="runtime" :genres="genres" :cast="cast" />
    </article>
</template>

<script>
import FullProd from './FullProd.vue';
import axios from 'axios';

export default {
    name: "SearchCard",
    props: {
        production: Object,
        type: String,
    },
    data() {
        return {
            baseImgUri: "https://image.tmdb.org/t/p",
            display: false,
            api: {
                key: "dc5cd34dec23a24fbe96764eb4a63f74",
                baseUri: "https://api.themoviedb.org/3",
            },
            seasons: "",
            runtime: null,
            genres: [],
            cast: [],
        }
    },
    components: { FullProd },
    methods: {
        hideFullProd(value) {
            this.display = value;
        },
        fetchDetails() {
            const { key, baseUri } = this.api;
            this.genres = [];
            this.cast = [];
            axios.get(`${baseUri}/${this.type}/${this.production.id}?api_key=${key}&language=it&append_to_response=credits`)
                .then(res => {
                    console.log(res.data.genres);
                    res.data.genres.forEach(genre => this.genres.push(genre.name));
                    res.data.credits.cast.forEach(item => this.cast.push(item.name));
                    if (this.type === "movie") {
                        this.runtime = res.data.runtime;
                    }
                    if (this.type === "tv") {
                        if (res.data.number_of_seasons === 1) {
                            this.seasons = res.data.number_of_seasons + " stagione";
                        }
                        else {
                            this.seasons = res.data.number_of_seasons + " stagioni";
                        }
                    }
                    this.display = true;
                });
        },
        getPoster() {
            if (!this.production.poster_path) return 'https://via.placeholder.com/342x513.png';
            return `${this.baseImgUri}/w342${this.production.poster_path}`;
        }
    }
}
</script>

<style lang="scss" scoped>
article {
    height: 100%;

    figure {
        height: 100%;
        margin: 0;
        position: relative;
        cursor: pointer;

        img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        h4 {
            position: absolute;
            top: 20px;
            left: 20px;
        }
    }
}
</style>