<script setup lang="ts">
import { defineEmits, defineProps, computed } from "vue";

export interface ICheckBoxProps {
  modelValue: boolean | string;
  value: boolean | string;
  label: string;
}

const props = defineProps<ICheckBoxProps>();

const emit = defineEmits<{
  (event: "update:modelValue", value: boolean | string): void;
}>();

const computedValue = computed({
  get(): boolean | string {
    return props.modelValue;
  },
  set(value: boolean | string) {
    emit("update:modelValue", value);
  },
});
</script>

<template>
  <label class="checkbox cursor-pointer">
    <input
      v-model="computedValue"
      type="checkbox"
      :value="value"
      class="checkbox__input cursor-pointer"
    />
    <span class="ml-5">{{ label }}</span>
  </label>
</template>

<style lang="scss">
.checkbox {
  align-items: center;
  display: flex;
  font-weight: 300;
  color: #707070;

  &__input {
    appearance: none;
    height: 1.125rem;
    min-height: 1.125rem;
    min-width: 1.125rem;
    outline: none !important;
    position: relative;
    width: 1.125rem;
    color: #232426;

    &::before {
      background-image: url(../../../assets/images/checkbox-empty.png);
      background-size: cover;
      bottom: 0;
      content: " ";
      display: inline-block;
      left: 0;
      position: absolute;
      right: 0;
      top: 0;
      -webkit-transition: 0.125s;
      transition: 0.125s;
    }

    &:checked::before {
      background-image: url(../../../assets/images/checkbox-checked.png);
    }
  }
}
</style>
