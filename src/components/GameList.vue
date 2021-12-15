<template>
  <div class="card">
    <div class="card-header">
      <div class="input-group">
        <Autocomplete :onChange="(data) => (search = data)" :options="games" />
        <select v-model="platform" class="form-control">
          <option value="null" selected>Filter Platform</option>
          <option v-for="platform of platforms" :value="platform">
            {{ platform }}
          </option>
        </select>
        <div class="input-group-append button-group">
          <button class="btn btn-primary" @click="toggleSortbyScore">
            Sort by Score ({{
              !sort ? 'default' : sort === 'asc' ? 'ASC' : 'DSEC'
            }})
          </button>
          <button class="btn btn-secondary" @click="clearFilter">
            Clear Filter
          </button>
        </div>
      </div>
    </div>
    <div class="card-body">
      <div class="row">
        <div
          class="col-lg-4 col-md-6"
          v-for="(game, index) of filteredGames"
          :key="index"
        >
          <GameCard :game="game" />
        </div>
      </div>
      <div v-if="filteredGames.length < 1" class="no-data-found">
        No game found.
      </div>
    </div>
  </div>
</template>
<script>
import GameCard from './GameCard.vue';
import Autocomplete from './ui/Autocomplete.vue';
export default {
  data() {
    return {
      search: '',
      sort: '',
      platform: null,
    };
  },
  components: {
    Autocomplete,
    GameCard,
  },
  props: {
    games: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    platforms() {
      return [...new Set(this.games.map((game) => game.platform))];
    },
    filteredGames() {
      let term = this.search ? this.search.toLowerCase() : '';
      let filtered = this.games.filter((game) => {
        let titleTerm = game.title.toLowerCase().includes(term);
        let platformTerm = this.platform
          ? game.platform === this.platform
          : true;
        return titleTerm && platformTerm;
      });
      let sorted = filtered.sort((a, b) => {
        if (this.sort && this.sort === 'asc') return a.score < b.score ? 1 : -1;
        else return 0;
      });

      return sorted;
    },
  },
  methods: {
    updateSearch(data) {
      this.search = data;
    },
    toggleSortbyScore() {
      this.sort = this.sort === 'asc' ? 'dsec' : 'asc';
    },
    clearFilter() {
      this.search = '';
      this.platform = null;
    },
  },
};
</script>
