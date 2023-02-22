<script>
import { ref } from "vue";
import Header from "../header/Header.vue";

const name = ref("");

export default {
  components: { Header },
  props: {
    openStartModal: {
      type: Boolean,
    },
    startGame: {
      type: Function,
    },
  },
};
</script>

<template>
  <div class="modal-overlay" v-if="this.openStartModal">
    <div class="modal">
      <div class="modal-screen">
        <Header />
        <p class="name-prompt-title">Player name:</p>
        <div class="name-prompt-container">
          <input v-model="name" placeholder="Enter name..." />
          <button @click="() => startGame(name)">Go!</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.name-prompt-title {
  color: #bcb1a8;
  font-size: 16px;
}

.name-prompt-container {
  display: flex;
  flex-direction: row;
  gap: 8px;
  align-items: center;
  justify-content: flex-start;
}

input {
  height: 40px;
  border-radius: 4px;
  border: 1px solid #bcb1a8;
  font-style: italic;
  padding: 6px;
}
</style>
