<template>
  <input v-model="value" :type="type || 'text'" />
  <span>
    {{ errorMessage }}
  </span>
</template>

<script setup>
import { useField } from "vee-validate";

const props = defineProps({
  name: String,
  type: String,
});

const { value, errorMessage } = useField(() => props.name);
</script>
