<script setup lang="ts">
import BaseInput from "@/components/atoms/base-input/BaseInput.vue";
import { defineProps, defineEmits, computed } from "vue";

const props = defineProps<{
  modelValue: string;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: string): void;
}>();

const computedValue = computed({
  get(): string {
    return props.modelValue;
  },
  set(value: string) {
    emit("update:modelValue", value);
  },
});
</script>

<template>
  <div class="px-10 pb-10 pt-5">
    <BaseInput v-model="computedValue" placeholder="Type to search" />
  </div>
</template>

<style></style>
