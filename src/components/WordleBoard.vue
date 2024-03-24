<script setup lang="ts">
import { VICTORY_MESSAGE, DEFEAT_MESSAGE } from "@/settings";
import englishWords from "@/englishWordsWith5Letters.json";
import { ref } from "vue";

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) =>
      wordGiven.length === 5 &&
      wordGiven.toUpperCase() === wordGiven &&
      englishWords.includes(wordGiven),
  },
});

const guessInProgress = ref("");
const guessSubmited = ref("");
</script>

<template>
  <input
    type="text"
    v-model="guessInProgress"
    @keydown.enter="guessSubmited = guessInProgress"
  />
  <p
    v-if="guessSubmited.length > 0"
    v-text="guessSubmited === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
  />
</template>
