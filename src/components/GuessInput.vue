<script setup lang="ts">
import { WORD_SIZE } from "@/settings";
import GuessView from "./GuessView.vue";
import englishWords from "@/englishWordsWith5Letters.json";
import { ref, computed } from "vue";

withDefaults(defineProps<{ disabled?: boolean }>(), { disabled: false });

const emit = defineEmits<{
  "guess-submitted": [guess: string];
}>();

const guessInProgress = ref<string | null>(null);

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
  },
});

function onSubmit() {
  if (!englishWords.includes(formattedGuessInProgress.value)) {
    return;
  }

  emit("guess-submitted", formattedGuessInProgress.value);

  guessInProgress.value = null;
}

function onInput(event: Event) {
  const input = event.target as HTMLInputElement;
  const newValue = input.value.replace(/[^A-Za-z]/g, "");
  if (newValue !== input.value) {
    input.value = newValue;
    formattedGuessInProgress.value = newValue;
  }
}
</script>

<template>
  <GuessView v-if="!disabled" :guess="formattedGuessInProgress" />

  <input
    v-model="formattedGuessInProgress"
    :maxlength="WORD_SIZE"
    :disabled="disabled"
    aria-label="Make your guess for the word of the day!"
    autofocus
    @blur="({target}) => (target as HTMLInputElement).focus()"
    type="text"
    @keydown.enter="onSubmit"
    @input="onInput"
  />
</template>

<style scoped>
input {
  position: absolute;
  opacity: 0;
}
</style>
