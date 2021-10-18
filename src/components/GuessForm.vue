<template>
  <el-input
    v-model="guess"
    @keypress="isNumber($event)"
    :input-style="inputStyle"
    ref="input"
    placeholder="Num"
    maxlength="4"
  />

  <el-button type="primary" class="btn guess-btn" @click="addGuess"
    >GUESS</el-button
  >
</template>

<script>
import { ref, computed, onMounted } from "vue";

export default {
  emits: ["add-guess"],
  setup(_, context) {
    const guess = ref("");
    const input = ref();

    const addGuess = () => {
      focusInput();

      if (guess.value.trim().length === 0) {
        return false;
      }

      context.emit("add-guess", guess);

      guess.value = "";
    };

    const focusInput = () => {
      input.value.focus();
    };

    const isNumber = (e) => {
      e = e ? e : window.event;
      var charCode = e.which ? e.which : e.keyCode;
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        e.preventDefault();
      } else {
        return true;
      }
    };

    const inputStyle = computed(() => {
      return {
        border: "2px solid #202020",
        borderRadius: "5px",
        width: "100px",
        height: "80px",
        fontSize: "28px",
        textAlign: "center",
        margin: "20px",
      };
    });

    onMounted(() => {
      focusInput();
    });

    return {
      guess,
      input,
      addGuess,
      isNumber,
      inputStyle,
    };
  },
};
</script>

<style scoped>
.guess-btn {
  width: 160px;
  padding: 15px 0;
  font-size: 18px;
  font-weight: 600;
}
</style>
