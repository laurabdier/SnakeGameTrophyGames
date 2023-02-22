<script>
export default {
  props: {
    leaderBoard: {
      type: Array,
      required: true,
    },
  },
};
</script>

<template>
  <table class="leader-board">
    <thead>
      <tr>
        <th>#</th>
        <th>Names</th>
        <th>Score</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(leader, i) in this.leaderBoard">
        <td>{{ i + 1 }}</td>
        <td>{{ leader.name }}</td>
        <td>{{ leader.score }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style>
.leader-board {
  background: #bcb1a8;
  border-radius: 10px;
}

thead {
  border-bottom: 1px solid #777fb4;
}

th {
  padding: 8px;
  text-align: start;
  font-weight: bold;
  color: #746d67;
}

td {
  padding: 8px;
  font-weight: bold;
  color: #ffffff;
}
</style>
