<template>
  <div>
    <MainHeader @search="startSearch" @select="getGenre" :genres-list="genresList" />
    <MainPage :films-list="filmsList" :shows-list="showsList" :search-term="searchTerm" />
  </div>
</template>

<script>
import axios from 'axios';
import MainHeader from './components/MainHeader.vue';
import MainPage from './components/MainPage.vue';

export default {
  name: 'App',
  components: {
    MainHeader,
    MainPage,
  },
  data() {
    return {
      genresList: [],
      filmsList: [],
      showsList: [],
      searchTerm: '',
      searchGenre: null,
      api: {
        key: "dc5cd34dec23a24fbe96764eb4a63f74",
        baseUri: "https://api.themoviedb.org/3",
        lang: 'it-IT'
      }
    }
  },
  methods: {
    getGenre(value) {
      this.searchGenre = value;
    },
    startSearch(value) {
      this.fetchData(value, '/search/movie', 'filmsList');
      this.fetchData(value, '/search/tv', 'showsList');
      this.searchTerm = value;
    },
    fetchData(value, endpoint, target) {
      const { key, baseUri, lang } = this.api;

      axios.get(`${baseUri}${endpoint}?api_key=${key}&language=${lang}&query=${value}`)
        .then(res => {
          if (this.searchGenre) {
            this[target] = res.data.results.filter(result => {
              if (result.genre_ids.includes(this.searchGenre)) return true;
              return false;
            });
          } else {
            this[target] = res.data.results;
          }
        });
    },
    fetchGenres(endpoint) {
      const { key, baseUri, lang } = this.api;

      axios.get(`${baseUri}${endpoint}?api_key=${key}&language=${lang}`)
        .then(res => {
          res.data.genres.forEach(genre => {
            this.genresList.push(genre);
          })
        });
    },
  },
  beforeMount() {
    this.fetchGenres('/genre/movie/list');
    this.fetchGenres('/genre/tv/list');
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';
</style>
