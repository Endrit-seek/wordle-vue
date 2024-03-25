<script setup lang="ts">
import { VICTORY_MESSAGE, DEFEAT_MESSAGE, WORD_SIZE } from "@/settings";
import englishWords from "@/englishWordsWith5Letters.json";
import { ref, computed } from "vue";

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven),
  },
});

const guessInProgress = ref<string | null>(null);
const guessSubmited = ref("");

const formattedGuessInProgress = computed<string>({
  get() {
    return guessInProgress.value ?? "";
  },
  set(rawValue: string) {
    guessInProgress.value = null;

    guessInProgress.value = rawValue
      .slice(0, WORD_SIZE)
      .toUpperCase()
      .replace(/[^A-Z]+/gi, "");
  }
});

function onSubmit() {
  if (!englishWords.includes(formattedGuessInProgress.value)) {
    return;
  }

  guessSubmited.value = formattedGuessInProgress.value;
}


// Event listener to prevent non-letter characters from being entered
function onInput(event: Event) {
  const input = event.target as HTMLInputElement;
  const newValue = input.value.replace(/[^A-Za-z]/g, '');
  if (newValue !== input.value) {
    input.value = newValue;
    formattedGuessInProgress.value = newValue;
  }
}
</script>

<template>
  <input
    v-model="formattedGuessInProgress"
    :maxlength="WORD_SIZE"
    type="text"
    @keydown.enter="onSubmit"
    @input="onInput"
  />
  <p
    v-if="guessSubmited.length > 0"
    v-text="guessSubmited === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
  />
</template>
