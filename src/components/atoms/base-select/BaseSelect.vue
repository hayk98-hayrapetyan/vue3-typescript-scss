<script setup lang="ts">
import { defineProps, defineEmits, ref, computed } from "vue";
import BaseInput from "@/components/atoms/base-input/BaseInput.vue";

export interface ISelectProps {
  value: string;
  modelValue: boolean;
  label: string;
}

const props = defineProps<{
  options: Array<ISelectProps>;
}>();

const emit = defineEmits<{
  (event: "select", value: number);
}>();

const inputValue = ref("");

const filteredOptions = computed(
  (): Array<ISelectProps> =>
    props.options.filter((option) =>
      option.label.toLowerCase().includes(inputValue.value.toLowerCase())
    )
);

const showOptions = computed(
  () => !!inputValue.value && filteredOptions.value.length
);

const selectLocation = (optionIndex: number) => {
  emit("select", optionIndex);

  inputValue.value = "";
};
</script>

<template>
  <div class="select">
    <BaseInput
      placeholder="Type to search"
      v-model="inputValue"
      class="select__input"
      :class="{ 'select__input--open': showOptions }"
    />

    <div v-if="showOptions" class="select__menu">
      <ul class="select__menu__results custom-scrollbar">
        <li
          v-for="(option, idx) in filteredOptions"
          :key="option.value"
          class="select__menu__results__item"
          :class="{
            'select__menu__results__item--selected': option.modelValue,
          }"
          @click="selectLocation(idx)"
        >
          {{ option.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style lang="scss">
.select {
  position: relative;

  &__input {
    &--open {
      border-end-end-radius: 0;
      border-end-start-radius: 0;
    }
  }

  &__menu {
    position: absolute;
    left: 0;
    top: 100%;
    width: 100%;
    z-index: 5;

    &__results {
      min-height: 36.8px;
      max-height: 184px;
      background-color: #fff;
      border: 1px solid hsla(0, 0%, 44%, 0.12);
      border-radius: 0 0 8px 8px !important;
      border-top: 0 solid hsla(0, 0%, 44%, 0.12);

      &__item {
        border-radius: 0;
        color: #232426;
        cursor: pointer;
        margin: 0;
        overflow: hidden;
        padding: 0.4rem 1rem;
        text-overflow: ellipsis;
        white-space: nowrap;
        width: 100%;

        &:hover,
        &--selected {
          background-color: #c4ffcf;
        }
      }
    }
  }
}
</style>
