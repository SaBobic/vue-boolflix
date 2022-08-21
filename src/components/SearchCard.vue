<template>
    <article>
        <figure @click="toggleFullProd">
            <img :src="`${baseImgUri}/w342${production.poster_path}`" :alt="production.title || production.name">
        </figure>
        <div class="full-prod d-none d-flex justify-content-center align-items-center" ref="fullProd">
            <div>
                <figure>
                    <img :src="`${baseImgUri}/w780${production.backdrop_path}`"
                        :alt="production.title || production.name">
                    <i class="fa-solid fa-xmark" @click="toggleFullProd"></i>
                    <div class="play-prod d-flex align-items-center">
                        <div class="play-button d-flex align-items-center">
                            <i class="fa-solid fa-play"></i>
                        </div>
                        <div>Riproduci</div>
                    </div>
                    <div class="gradient-overlay"></div>
                </figure>
                <div class="prod-info">
                    <figcaption class="title">{{ production.title || production.name }}</figcaption>
                    <div class="vote">
                        <i v-for="index in correctVote" class="fa-solid fa-star" :key="index"></i>
                        <span class="release-date">{{ year }}</span>
                        <span class="runtime">{{ formattedRuntime }}</span>
                        <span class="seasons">{{ seasons }}</span>
                    </div>
                    <div class="overview">{{ reducedOverview }}</div>
                    <div class="production-language">
                        Lingua originale: <img
                            v-if="production.original_language === 'it' || production.original_language === 'en'"
                            :src="require(`../assets/img/${production.original_language}.png`)" alt="">
                        <span v-else>{{ production.original_language }}</span>
                    </div>
                    <div v-if="genres.length > 0" class="genres">Genere: {{ detailsList(genres) }}</div>
                    <div v-if="cast.length > 0" class="cast">Cast: {{ detailsList(cast) }}</div>
                </div>
            </div>
        </div>
    </article>
</template>

<script>
import axios from 'axios';

export default {
    name: 'SearchCard',
    props: {
        production: Object,
        type: String,
    },
    data() {
        return {
            baseImgUri: "https://image.tmdb.org/t/p",
            api: {
                key: "dc5cd34dec23a24fbe96764eb4a63f74",
                baseUri: "https://api.themoviedb.org/3",
            },
            genres: [],
            cast: [],
            runtime: null,
            seasons: '',
        };
    },
    computed: {
        reducedOverview() {
            const supString = this.production.overview;
            return supString.length > 250 ? supString.substr(0, 250).trim() + '...' : supString;
        },
        correctVote() {
            return Math.ceil(this.production.vote_average * 0.5);
        },
        formattedRuntime() {
            const hours = Math.floor(this.runtime / 60);
            const minutes = this.runtime - (hours * 60);
            return `${hours}h ${minutes}min`;
        },
        year() {
            if (this.production.release_date) {
                const d = new Date(this.production.release_date);
                return d.getFullYear();
            } else {
                const d = new Date(this.production.first_air_date);
                return d.getFullYear();
            }
        },
    },
    methods: {
        toggleFullProd() {
            this.$refs.fullProd.classList.toggle('d-none');
        },
        fetchDetails() {
            const { key, baseUri } = this.api;

            axios.get(`${baseUri}/${this.type}/${this.production.id}?api_key=${key}&language=it&append_to_response=credits`)
                .then(res => {
                    res.data.genres.forEach(genre => this.genres.push(genre.name));
                    res.data.credits.cast.forEach(item => this.cast.push(item.name));
                    if (this.type === 'movie') {
                        this.runtime = res.data.runtime;
                    }
                    if (this.type === 'tv') {
                        if (res.data.number_of_seasons === 1) {
                            this.seasons = res.data.number_of_seasons + ' stagione';
                        } else {
                            this.seasons = res.data.number_of_seasons + ' stagioni';
                        }
                    }
                });
        },
        detailsList(arr) {
            let string = '';
            for (let i = 0; i < arr.length && i < 5; i++) {
                i === arr.length - 1 || i === 4 ? string += `${arr[i]}` : string += `${arr[i]}, `;
            }
            return string;
        },
    },
    created() {
        this.fetchDetails();
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


                i.fa-xmark {
                    font-size: 28px;
                    text-shadow: 0px 0px 5px #000;
                    position: absolute;
                    right: 10px;
                    top: 10px;
                    cursor: pointer;
                }

                .gradient-overlay {
                    height: 20px;
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    right: 0;
                    background: linear-gradient(transparent 0%, #181818 100%);
                }

                .play-prod {
                    cursor: pointer;
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    transform: translateY(30%);
                    margin-left: 50px;
                    font-weight: 700;
                    z-index: 1;

                    .play-button {
                        font-size: 19px;
                        width: 40px;
                        height: 40px;
                        background-color: #000;
                        border: 2px solid #fff;
                        border-radius: 50%;
                        margin-right: 8px;

                        i {
                            color: #fff;
                            transform: translate(13px, -1px);
                        }
                    }

                    &:hover {
                        .play-button {
                            border: 2px solid #DE0913;
                            background-color: #DE0913;
                        }
                    }
                }
            }

            .prod-info {
                padding: 30px 50px;

                figcaption {
                    font-size: 30px;
                    font-weight: 700;
                }

                figcaption,
                .vote,
                .overview,
                .production-language {
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

                .release-date,
                .runtime {
                    margin-right: 10px;
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