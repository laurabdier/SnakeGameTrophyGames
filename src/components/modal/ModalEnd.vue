<script>
import Header from "../header/Header.vue";
import LeaderBoard from "../LeaderBoard.vue";

export default {
  components: { LeaderBoard, Header },
  props: {
    openEndModal: {
      type: Boolean,
    },
    playerName: {
      type: String,
    },
    leaderBoard: {
      type: Array,
    },
    resetGame: {
      type: Function,
    },
    failMessage: {
      type: String,
    },
  },
};
</script>

<template>
  <div class="modal-overlay" v-if="this.openEndModal">
    <div class="modal">
      <div class="modal-screen" style="align-items: center">
        <Header />
        <p>{{ this.failMessage }}</p>
        <p>{{ playerName }}</p>
        <LeaderBoard :leaderBoard="leaderBoard" />
        <div>
          <button @click="() => this.resetGame(false)">Play again</button>
          <button @click="() => this.resetGame(true)">New player</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
p {
  color: #746d67;
  font-size: 16px;
}
</style>
