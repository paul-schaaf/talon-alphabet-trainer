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

<script lang="ts">
import { defineComponent, onMounted, Ref, ref } from "vue";
import * as ProgressBar from "progressbar.js";

export default defineComponent({
  name: "App",
  setup() {
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
    let bar: any = null;
    onMounted(() => {
      letter.value = getLetter();
      input.value.focus();
      const letterInterval = setInterval(() => {
        bar.destroy();
        if (letter.value === inputLetter.value) {
          letter.value = getLetter();
          input.value.focus();
          inputLetter.value = "";
          counter += 1;
          bar = createProgressBar();
        } else {
          clearInterval(letterInterval);
          letter.value = counter;
        }
      }, 2000);

      bar = createProgressBar();
    });

    return { letter, input, inputLetter };
  },
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
