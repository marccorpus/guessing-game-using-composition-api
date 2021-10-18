<template>
  <div>
    <h3 v-if="remarks">{{ remarks }}</h3>

    <p>No. of guesses: {{ guesses.length }}</p>

    <p>
      Guessed numbers are:
      <el-tag
        v-for="guess in guesses"
        :key="guess.id"
        :type="guess.isCorrect ? 'success' : 'danger'"
        effect="dark"
        class="tags"
        >{{ guess.number }}</el-tag
      >
    </p>
  </div>
</template>

<script>
import { computed } from "vue";

export default {
  props: ["randomNumber", "guesses"],
  setup(props) {
    const remarks = computed(() => {
      let remarks = null;

      const length = props.guesses.length;

      if (length > 0) {
        if (props.guesses[length - 1].number === props.randomNumber) {
          remarks = "Yippie You Win!";
        } else if (props.guesses[length - 1].number > props.randomNumber) {
          remarks = "Your guess is too high.";
        } else if (props.guesses[length - 1].number < props.randomNumber) {
          remarks = "Your guess is too low.";
        }
      }

      return remarks;
    });

    return {
      remarks,
    };
  },
};
</script>

<style scoped>
div {
  margin-top: 30px;
}
.tags {
  margin-left: 10px;
  margin-bottom: 10px;
  font-weight: 600;
}
</style>
