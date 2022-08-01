<template>
    <article>
        <figure @click="showFullProd">
            <img :src="`${baseImgUri}/w342${production.poster_path}`" :alt="production.title || production.name">
            <figcaption>{{ production.title || production.name }}</figcaption>
        </figure>
        <div class="full-prod d-none d-flex justify-content-center align-items-center" ref="fullProd">
            <div>
                <figure>
                    <img :src="`${baseImgUri}/w780${production.backdrop_path}`"
                        :alt="production.title || production.name">
                    <figcaption class="title">{{ production.title || production.name }}</figcaption>
                    <i class="fa-solid fa-xmark" @click="hideFullProd"></i>
                </figure>
                <div class="prod-info">
                    <div class="vote">
                        <i v-for="index in getCorrectVote(production.vote_average)" class="fa-solid fa-star"
                            :key="index"></i>
                        <span>{{ production.release_date || production.first_air_date }}</span>
                    </div>
                    <div class="overview">{{ reducedOverview }}</div>
                    <div class="production-language">
                        Lingua originale: <img
                            v-if="production.original_language === 'it' || production.original_language === 'en'"
                            :src="require(`../assets/img/${production.original_language}.png`)" alt="">
                        <span v-else>{{ production.original_language }}</span>
                    </div>
                </div>
            </div>
        </div>
    </article>
</template>

<script>
export default {
    name: 'SearchCard',
    props: {
        production: Object,
    },
    data() {
        return {
            baseImgUri: "https://image.tmdb.org/t/p",
        };
    },
    computed: {
        reducedOverview() {
            const supString = this.production.overview;
            return supString.length > 150 ? supString.substr(0, 150) + '...' : supString;
        },
    },
    methods: {
        getCorrectVote(number) {
            return Math.ceil(number * 0.5);
        },
        showFullProd() {
            this.$refs.fullProd.classList.remove('d-none');
            this.$refs.fullProd.classList.add('d-block');
        },
        hideFullProd() {
            this.$refs.fullProd.classList.remove('d-block');
            this.$refs.fullProd.classList.add('d-none');
        },
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

        figcaption {
            background-color: rgba($color: #000000, $alpha: 0.6);
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            padding: 5px 10px;
            font-size: 17px;
            font-weight: 700;
        }
    }

    .full-prod {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 1;
        background-color: rgba($color: #000, $alpha: 0.9);

        >div {
            width: 800px;
            background-color: #181818;

            figure {
                position: relative;
                margin-bottom: 0;

                img {
                    width: 100%;
                }

                figcaption {
                    font-size: 30px;
                    font-weight: 700;
                    position: absolute;
                    left: 0;
                    right: 0;
                    bottom: 0;
                    padding: 10px 50px;
                    background: linear-gradient(transparent 0%, #181818 100%);
                }

                i {
                    font-size: 28px;
                    text-shadow: 0px 0px 5px #000;
                    position: absolute;
                    right: 10px;
                    top: 10px;
                    cursor: pointer;
                }
            }

            .prod-info {
                padding: 0 50px 30px;

                .vote,
                .overview {
                    margin-bottom: 10px;
                }

                .vote {
                    i {
                        color: #0ED145;

                        &:last-of-type {
                            margin-right: 10px;
                        }
                    }
                }

                .production-language {
                    img {
                        height: 16px;
                    }
                }
            }


        }
    }
}
</style>