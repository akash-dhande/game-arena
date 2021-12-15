<template>
  <div class="card game-card">
    <div class="card-body">
      <div class="card-title">{{ game.title }}</div>
      <div>
        Platform: <strong>{{ game.platform }}</strong>
      </div>
      <div>
        Genre: <strong>{{ game.genre }}</strong>
      </div>
    </div>
    <div class="card-footer">
      <div>
        Score:
        <span class="badge badge-primary">{{ game.score }}</span>
      </div>
      <div class="badge badge-success" v-if="game.editors_choice === 'Y'">
        Editors Choice
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    game: {
      type: Object,
      default: () => {},
    },
  },
};
</script>
