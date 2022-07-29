<template>
  <div>
    <MainHeader @search="startSearch" />
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
      api: {
        key: "dc5cd34dec23a24fbe96764eb4a63f74",
        baseUri: "https://api.themoviedb.org/3",
        lang: 'it-IT'
      }
    }
  },
  methods: {
    startSearch(value) {
      this.fetchData(value, '/search/movie', 'filmsList');
      this.fetchData(value, '/search/tv', 'showsList');
    },
    fetchData(value, endpoint, target) {
      const { key, baseUri, lang } = this.api;

      axios.get(`${baseUri}${endpoint}?api_key=${key}&language=${lang}&query=${value}`)
        .then(res => {
          this[target] = res.data.results;
        });
    }
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';
</style>
