<template>
  <input v-model="inputLetter" ref="input" type="text" style="opacity: 0" />
  <div style="display: flex; justify-content: center">
    <div style="border: 5px solid orange; border-radius: 8px; width: 300px">
      <p style="font-size: 80px">{{ letter }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, Ref, ref } from "vue";

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

    onMounted(() => {
      letter.value = getLetter();
      input.value.focus();
      const letterInterval = setInterval(() => {
        if (letter.value === inputLetter.value) {
          letter.value = getLetter();
          input.value.focus();
          inputLetter.value = "";
          counter += 1;
        } else {
          clearInterval(letterInterval);
          letter.value = counter;
        }
      }, 2000);
    });

    return { letter, input, inputLetter };
  },
});
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
