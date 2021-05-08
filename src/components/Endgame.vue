<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="endgame">
      <div class="highScore">
        <h3>The Ruler</h3>
        <h4>David With: {{ highestScore }}</h4>
      </div>
      <h1>Game over</h1>
      <h2>Your score is: {{ score }}</h2>
      <p style="margin-top: -20px">{{ scoreInWords }}</p>
      <p>Total game time: {{ time }} seconds</p>
      <button class="buttonStyle" @click="closeModal">new game</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: ["score", "time", "scoreInWords", "highestScore"],
  methods: {
    closeModal() {
      this.$emit("closeEndGame");
    },
  },
};
</script>

<style scoped>
.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
.endgame {
  width: 400px;
  padding: 20px;
  margin: 140px auto;
  background: white;
  border-radius: 10px;
}

.endgame h1 {
  margin-top: -40px;
  color: blueviolet;
  border-bottom: 5px solid blueviolet;
  padding: 0;
  /* display: inline-block; */
}
.endgame p {
  font-style: normal;
}
.highScore {
  padding: 0px;
  position: relative;
  bottom: 35px;
  left: 160px;
  color: #f6b93b;
}
.highScore h4 {
  margin: -20px;
}
</style>