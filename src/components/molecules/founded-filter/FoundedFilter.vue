<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import FilterModalSection from "@/components/molecules/filter-modal-section/FilterModalSection.vue";
import BaseInput from "@/components/atoms/base-input/BaseInput.vue";

export type FoundedYearProp = {
  start: string;
  end: string;
};

defineProps<{
  filters: FoundedYearProp;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: FoundedYearProp): void;
}>();

const updateValue = (data: FoundedYearProp) => {
  emit("update:modelValue", data);
};
</script>

<template>
  <FilterModalSection title="FOUNDED YEAR">
    <div class="founded">
      <BaseInput
        class="founded__input"
        placeholder="2010"
        :max="4"
        :model-value="filters.start"
        @update:model-value="updateValue({ start: $event })"
      />
      <span>to</span>
      <BaseInput
        class="founded__input"
        placeholder="2022"
        :max="4"
        :model-value="filters.end"
        @update:model-value="updateValue({ end: $event })"
      />
    </div>
  </FilterModalSection>
</template>

<style lang="scss">
.founded {
  display: flex;
  align-items: center;
  gap: 30px;

  &__input {
    width: 100px;
  }
}
</style>
