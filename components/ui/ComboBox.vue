<template>
  <select v-model="value">
    <option v-for="item in items" :key="item.id" :value="item.id">
      {{ item[itemText] }}
    </option>
  </select>
  <span>
    {{ errorMessage }}
  </span>
</template>

<script setup lang="ts">
import { useField } from "vee-validate";
import type { PropType } from "vue";

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  items: {
    type: Array as PropType<any[]>,
    required: true,
  },
  itemText: {
    type: String,
    required: true,
  },
});

const { value, errorMessage } = useField(() => props.name);
</script>
