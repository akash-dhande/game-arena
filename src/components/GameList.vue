<template>
  <div class="card">
    <div class="card-header">
      <div class="input-group">
        <input
          v-model="search"
          placeholder="Search Game by Name"
          class="form-control"
          type="search"
        />
        <select v-model="platform" class="form-control">
          <option value="null" selected>Filter Platform</option>
          <option v-for="platform of platforms" :value="platform">
            {{ platform }}
          </option>
        </select>
        <div class="input-group-append">
          <button class="btn btn-primary" @click="toggleSortbyScore">
            Sort by Score ({{
              !sort ? 'default' : sort === 'asc' ? 'ASC' : 'DSEC'
            }})
          </button>
        </div>
      </div>
    </div>
    <div class="card-body">
      <div class="row">
        <div
          class="col-md-4 col-sm-6"
          v-for="(game, index) of filteredGames"
          :key="index"
        >
          <GameCard :game="game" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import GameCard from './GameCard.vue';
export default {
  data() {
    return {
      search: '',
      sort: '',
      platform: null,
    };
  },
  components: {
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
        let titleTerm = game.title && game.title.toLowerCase().includes(term);
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
    toggleSortbyScore() {
      this.sort = this.sort === 'asc' ? 'dsec' : 'asc';
    },
  },
};
</script>
