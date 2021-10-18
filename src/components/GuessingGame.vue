<template>
  <el-row>
    <el-col :span="16" :offset="4">
      <el-card class="card">
        <h3>I am thinking of a number between 1-1000.</h3>
        <h3>Can you guess it?</h3>
        <el-divider><i class="el-icon-star-on"></i></el-divider>

        <el-button v-if="!playing" type="primary" class="btn" @click="startGame"
          >Start Game</el-button
        >

        <template v-else>
          <GuessForm v-if="!winner" @add-guess="addGuess" />

          <Guesses :random-number="randomNumber" :guesses="guesses" />

          <el-button
            v-if="winner"
            type="primary"
            class="btn"
            @click="restartGame"
            >Restart Game</el-button
          >
        </template>
      </el-card>
    </el-col>
  </el-row>
</template>

<script>
import { ref } from "vue";

import GuessForm from "./GuessForm.vue";
import Guesses from "./Guesses.vue";

export default {
  setup() {
    const playing = ref(false);
    const winner = ref(false);
    const randomNumber = ref(null);
    const guesses = ref([]);

    const generateRandomNumber = () => {
      return Math.floor(Math.random() * 1000) + 1;
    };

    const startGame = () => {
      playing.value = true;
      randomNumber.value = generateRandomNumber();
    };

    const restartGame = () => {
      winner.value = false;
      randomNumber.value = generateRandomNumber();
      guesses.value = [];
    };

    const checkWinner = (guess) => {
      if (randomNumber.value === +guess.value) {
        winner.value = true;
      }
    };

    const addGuess = (guess) => {
      guesses.value = [
        ...guesses.value,
        {
          id: new Date().toISOString(),
          number: +guess.value,
          isCorrect: randomNumber.value === +guess.value,
        },
      ];

      checkWinner(guess);
    };

    return {
      playing,
      winner,
      randomNumber,
      guesses,
      startGame,
      restartGame,
      addGuess,
    };
  },
  components: {
    GuessForm,
    Guesses,
  },
};
</script>

<style scoped>
.card {
  border-radius: 5px !important;
}
</style>
