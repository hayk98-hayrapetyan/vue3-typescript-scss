<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps<{
  modelValue?: string;
  name?: string;
  placeholder: string;
  max?: number;
}>();

const emit = defineEmits(["update:modelValue"]);

const input = ref<null | HTMLElement>(null);

const updateValue = (e: Event) => {
  const target = e.target as HTMLInputElement;
  const value = target.value.trim();

  if (props.max && value.length > props.max) {
    input.value.value = props.modelValue;
    return emit("update:modelValue", props.modelValue);
  }

  emit("update:modelValue", value.trim());
};
</script>

<template>
  <input
    type="text"
    class="input"
    :value="modelValue"
    :name="name"
    :placeholder="placeholder"
    ref="input"
    @input="updateValue"
  />
</template>

<style lang="scss">
.input {
  appearance: none;
  background-color: #fff;
  border: 1px solid hsla(0, 0%, 44%, 0.12);
  border-radius: 1rem;
  height: 2rem;
  padding: 0 1rem;
  color: $black;
  width: 100%;

  &:hover,
  &:focus {
    outline: none;
    background-color: #fff;
    border: 1px solid hsla(0, 0%, 44%, 0.12);
  }

  &::placeholder {
    opacity: 0.7;
    font-weight: 500;
  }
}
</style>
