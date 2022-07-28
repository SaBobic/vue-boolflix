<template>
  <div>
    <input type="text" placeholder="Cerca un film" v-model="userInputText" @keyup.enter="getArticlesLists">
    <button @click="getArticlesLists">CERCA</button>

    <h2>Films</h2>

    <ul>
      <li v-for="film in filmsList" :key="film.id">
        <h4>{{ film.title }}</h4>
        <ul>
          <li><strong>Titolo originale</strong>: {{ film.original_title }}</li>
          <li v-if="film.poster_path"><strong>Copertina</strong>: <img :src="`${baseImgUri}/w342${film.poster_path}`"
              :alt="film.title"></li>
          <li v-if="film.original_language === 'it' || film.original_language === 'en'"><strong>Lingua</strong>: <img
              :src="require(`./assets/img/${film.original_language}.png`)" alt=""></li>
          <li v-else><strong>Lingua</strong>: {{ film.original_language }}</li>
          <li v-if="film.vote_average">
            <strong>Voto</strong>:
            <i v-for="index in getCorrectVote(film.vote_average)" class="fa-solid fa-star" :key="index"></i>
          </li>
        </ul>
      </li>
    </ul>

    <hr>

    <h2>TV Shows</h2>

    <ul>
      <li v-for="show in showsList" :key="show.id">
        <h4>{{ show.name }}</h4>
        <ul>
          <li><strong>Titolo originale</strong>: {{ show.original_name }}</li>
          <li v-if="show.poster_path"><strong>Copertina</strong>: <img :src="`${baseImgUri}/w342${show.poster_path}`"
              :alt="show.name"></li>
          <li v-if="show.original_language === 'it' || show.original_language === 'en'"><strong>Lingua</strong>: <img
              :src="require(`./assets/img/${show.original_language}.png`)" alt=""></li>
          <li v-else><strong>Lingua</strong>: {{ show.original_language }}</li>
          <li v-if="show.vote_average">
            <strong>Voto</strong>:
            <i v-for="index in getCorrectVote(show.vote_average)" class="fa-solid fa-star" :key="index"></i>
          </li>
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
      baseImgUri: 'https://image.tmdb.org/t/p',
      apiKey: 'dc5cd34dec23a24fbe96764eb4a63f74',
      searchFilmsEndpoint: '/search/movie',
      searchShowsEndpoint: '/search/tv',
      userInputText: '',
      filmsList: '',
      showsList: '',
    }
  },
  methods: {
    getArticlesLists() {
      axios.get(`${this.baseUri}${this.searchFilmsEndpoint}?api_key=${this.apiKey}&query=${this.userInputText}`)
        .then(res => {
          this.filmsList = res.data.results;
        });
      axios.get(`${this.baseUri}${this.searchShowsEndpoint}?api_key=${this.apiKey}&query=${this.userInputText}`)
        .then(res => {
          this.showsList = res.data.results;
        });
      this.userInputText = '';
    },
    getCorrectVote(number) {
      return Math.ceil(number * 0.5);
    }
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';
</style>
