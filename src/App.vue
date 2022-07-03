<template>
  <input
    v-model="inputLetter"
    ref="input"
    type="text"
    style="opacity: 0; position: absolute"
  />
  <div
    style="
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
    "
  >
    <p v-if="gameOver">Your score is:</p>
    <div
      style="
        border: 5px solid orange;
        border-radius: 8px;
        width: 300px;
        padding: 50px 0 50px 0;
      "
    >
      <p style="font-size: 80px">{{ letter }}</p>
    </div>
    <div
      style="
        width: 295px;
        margin-top: 10px;
        height: 15px;
        border-radius: 50px;
        border: 1px solid transparent;
        overflow-x: hidden;
        overflow-y: hidden;
      "
    >
      <div
        id="container"
        style="width: 300px; transform: translate(-2.5px)"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, Ref, ref, watch } from "vue";
import * as ProgressBar from "progressbar.js";

const alphabet = [
  "a",
  "b",
  "c",
  "d",
  "e",
  "f",
  "g",
  "h",
  "i",
  "j",
  "k",
  "l",
  "m",
  "n",
  "o",
  "p",
  "q",
  "r",
  "s",
  "t",
  "u",
  "v",
  "w",
  "x",
  "y",
  "z",
];
const createProgressBar = () => {
  const bar = new ProgressBar.Line("#container", {
    strokeWidth: 4,
    easing: "linear",
    duration: 2000,
    color: "blue",
    trailColor: "#eee",
    svgStyle: { width: "100%", height: "100%" },
  });
  bar.animate(1.0);
  return bar;
};
let counter = 0;
const getLetter = () => {
  // eslint-disable-next-line no-constant-condition
  while (true) {
    const newLetter = alphabet[Math.floor(Math.random() * 26)];
    if (newLetter !== letter.value) {
      return newLetter;
    }
  }
};
const letter = ref("") as Ref<string | number>;
const inputLetter = ref("");
const input = ref(null as unknown as HTMLElement);
// eslint-disable-next-line @typescript-eslint/no-explicit-any
let bar: any = null;
// eslint-disable-next-line @typescript-eslint/no-explicit-any
let letterInterval: any = null;
let gameOver = ref(false);

watch(inputLetter, (newLetter) => {
  if (gameOver.value !== true) {
    if (newLetter !== "") {
      if (newLetter === letter.value) {
        clearTimeout(letterInterval);
        bar.destroy();
        letter.value = getLetter();
        input.value.focus();
        inputLetter.value = "";
        counter += 1;
        bar = createProgressBar();
      } else {
        clearTimeout(letterInterval);
        letter.value = counter;
        gameOver.value = true;
        bar.destroy();
      }
      letterInterval = setTimeout(() => {
        if (newLetter !== letter.value) {
          clearTimeout(letterInterval);
          letter.value = counter;
          gameOver.value = true;
          bar.destroy();
        }
      }, 2000);
    }
  }
});

onMounted(() => {
  input.value.focus();
  letter.value = getLetter();
  inputLetter.value = "";
  bar = createProgressBar();
  letterInterval = setTimeout(() => {
    if (inputLetter.value !== letter.value) {
      clearTimeout(letterInterval);
      letter.value = counter;
      gameOver.value = true;
      bar.destroy();
    }
  }, 2000);
});
</script>
<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
