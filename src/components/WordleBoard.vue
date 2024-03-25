<script setup lang="ts">
import { VICTORY_MESSAGE, DEFEAT_MESSAGE } from "@/settings";
import englishWords from "@/englishWordsWith5Letters.json";
import { ref } from "vue";
import GuessInput from "./GuessInput.vue";

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven),
  },
});

const guessSubmited = ref("");
</script>

<template>
  <GuessInput @guess-submitted="(guess) => (guessSubmited = guess)" />
  <p
    v-if="guessSubmited.length > 0"
    v-text="guessSubmited === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
  />
</template>
