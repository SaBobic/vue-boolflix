<template>
  <div>
    <input type="text" placeholder="Cerca un film" v-model="userInputText">
    <button @click="getFilmsList">CERCA</button>

    <ul>
      <li v-for="film in filmsList" :key="film.id">
        <h4>{{ film.title }}</h4>
        <ul>
          <li><strong>Titolo originale</strong>: {{ film.original_title }}</li>
          <li><strong>Lingua</strong>: {{ film.original_language }}</li>
          <li><strong>Voto</strong>: {{ film.vote_average }}</li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      baseUri: 'https://api.themoviedb.org/3',
      apiKey: 'dc5cd34dec23a24fbe96764eb4a63f74',
      searchFilmEndpoint: '/search/movie',
      userInputText: '',
      filmsList: '',
    }
  },
  methods: {
    getFilmsList() {
      axios.get(`${this.baseUri}${this.searchFilmEndpoint}?api_key=${this.apiKey}&query=${this.userInputText}`)
        .then(res => {
          this.filmsList = res.data.results;
        })
    }
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';
</style>
