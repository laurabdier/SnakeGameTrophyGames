<script setup>
import { ref } from "vue";
import LeaderBoard from "./LeaderBoard.vue";
import SnakeGameCanvas from "./SnakeGameCanvas.vue";
import Header from "./header/Header.vue";
import ModalStart from "./modal/ModalStart.vue";
import ModalEnd from "./modal/ModalEnd.vue";

const openStartModal = ref(true);
function setOpenStartModal(value) {
  openStartModal.value = value;
}

const openEndModal = ref(false);
function setOpenEndModal(value) {
  openEndModal.value = value;
}

const failMessage = ref("");
function setFailMessage(value) {
  failMessage.value = value;
}

const playerName = ref(null);
function setPlayerName(value) {
  playerName.value = value;
}

const canStartGame = ref(false);
function setCanStartGame(value) {
  canStartGame.value = value;
}

const playerScore = ref(0);
function setPlayerScore(value) {
  playerScore.value = value;
}

const timer = ref(null);
const timePassed = ref(0);
function setTimePassed(value) {
  timePassed.value = value;
}
function startTimer() {
  timer.value = setInterval(() => (timePassed.value += 1), 1000);
}
function stopTimer() {
  clearInterval(timer.value);
}

function formatTime(time) {
  const minutes = Math.floor(time / 60);
  let seconds = time % 60;

  if (seconds < 10) {
    seconds = `0${seconds}`;
  }

  return `${minutes}:${seconds}`;
}

const leaderBoard = ref([]);

function startGame(playerName) {
  if (!playerName) {
    return;
  }

  setPlayerName(playerName);
  setOpenStartModal(false);
  startTimer();
  setCanStartGame(true);
}

function endGame(failMessage) {
  setCanStartGame(false);
  setFailMessage(failMessage);
  leaderBoard.value.push({
    name: playerName.value,
    score: playerScore.value,
  });

  leaderBoard.value = leaderBoard.value
    .sort((a, b) => {
      if (a.score >= b.score) return -1;
      return 1;
    })
    .slice(0, 10);

  stopTimer();
  setOpenEndModal(true);
}

function resetGame(newPlayer) {
  setPlayerScore(0);
  setTimePassed(0);
  if (newPlayer) {
    setPlayerName(null);
    setOpenStartModal(true);
  }

  setCanStartGame(true);
  setOpenEndModal(false);
}
</script>

<template>
  <div class="game-page">
    <Header />
    <div class="game-page-content" v-if="!!playerName">
      <div class="game-page-element">
        <div class="game-page-element" v-if="playerName">
          <div class="player-info" style="font-weight: bold">
            {{ playerName }}
          </div>
          <div class="player-info">{{ playerScore }}</div>
          <div class="timer">{{ formatTime(timePassed) }}</div>
        </div>
      </div>
      <div class="game-page-element">
        <SnakeGameCanvas
          :canStartGame="canStartGame"
          :setPlayerScore="setPlayerScore"
          :endGame="endGame"
          :score="playerScore"
          :setTimePassed="(time) => (timePassed.value = time)"
          :stopTimer="stopTimer"
        />
      </div>
      <div class="game-page-element">
        <LeaderBoard :leaderBoard="leaderBoard" />
      </div>
    </div>
    <ModalStart :openStartModal="openStartModal" :startGame="startGame" />
    <ModalEnd
      :openEndModal="openEndModal"
      :playerName="playerName"
      :leaderBoard="leaderBoard"
      :resetGame="resetGame"
      :failMessage="failMessage"
    />
  </div>
</template>

<style>
.game-page {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
  gap: 14px;
}

.game-page-content {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: row;
  justify-content: space-between;
}

.game-page-element {
  width: 100%;
  align-items: center;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.player-info {
  font-size: 18px;
  color: #746d67;
}

.timer {
  font-size: 32px;
  font-weight: 800;
  color: #746d67;
  border: 2px solid #bcb1a8;
  border-radius: 4px;
  padding: 6px;
  width: 70%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
