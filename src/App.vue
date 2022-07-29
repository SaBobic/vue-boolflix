<template>
  <div>
    <MainHeader @search="fetchArticlesLists" />
    <MainPage :films-list="filmsList" :shows-list="showsList" />
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
      filmsList: [],
      showsList: [],
      apiKey: "dc5cd34dec23a24fbe96764eb4a63f74",
      baseUri: "https://api.themoviedb.org/3",
    }
  },
  methods: {
    fetchArticlesLists(value) {
      axios.get(`${this.baseUri}/search/movie?api_key=${this.apiKey}&query=${value}`)
        .then(res => {
          this.filmsList = res.data.results;
        });
      axios.get(`${this.baseUri}/search/tv?api_key=${this.apiKey}&query=${value}`)
        .then(res => {
          this.showsList = res.data.results;
        });
    },
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';
</style>
