<script setup lang="ts">
import { VICTORY_MESSAGE, DEFEAT_MESSAGE } from "@/settings";
import { ref } from "vue";

defineProps<{
  wordOfTheDay: string;
}>();

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
