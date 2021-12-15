<template>
  <div id="app">
    <div class="container">
      <h1>Game Arena</h1>
      <GameList v-if="!loading && !error" :games="games" />
      <div v-if="loading">Loading Games Please Wait...</div>
      <div v-if="error">Something Went Wrong, Please Try Again!</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import GameList from './components/GameList.vue';

export default {
  name: 'App',
  data() {
    return {
      loading: false,
      error: false,
      games: [],
    };
  },
  components: {
    GameList,
  },
  created() {
    this.fetchGames();
  },
  methods: {
    async fetchGames() {
      this.loading = true;
      try {
        let response = await axios.get(
          'https://s3-ap-southeast-1.amazonaws.com/he-public-data/gamesarena274f2bf.json'
        );
        this.games = response.data.filter((game) => game.title);
        this.loading = false;
      } catch {
        this.loading = false;
        this.error = true;
      }
    },
  },
};
</script>

<style lang="scss">
@import '~bootstrap/dist/css/bootstrap.css';
@import './assets/styles/style.scss';
</style>
