<template>
    <article>
        <figure @click="display = true">
            <img :src="`${baseImgUri}/w342${production.poster_path}`" :alt="production.title || production.name">
        </figure>
        <FullProd v-if="display" :production="production" :base-img-uri="baseImgUri" @display="hideFullProd" />
    </article>
</template>

<script>
import FullProd from './FullProd.vue';

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
        };
    },
    components: { FullProd },
    methods: {
        hideFullProd(value) {
            this.display = value;
        }
    }
}
</script>

<style lang="scss" scoped>
article {
    height: 100%;

    >figure {
        height: 100%;
        margin: 0;
        position: relative;
        cursor: pointer;

        img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }
}
</style>