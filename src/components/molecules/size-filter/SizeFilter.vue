<script setup lang="ts">
import BaseCheckbox, {
  ICheckBoxProps,
} from "@/components/atoms/base-checkbox/BaseCheckbox.vue";
import { defineProps, defineEmits } from "vue";
import FilterModalSection from "@/components/molecules/filter-modal-section/FilterModalSection.vue";

export type SizeFilterEmit = { filterIndex: number; value: boolean };

defineProps<{
  filters: Array<ICheckBoxProps>;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: SizeFilterEmit): void;
}>();

const updateValue = (data: SizeFilterEmit) => {
  emit("update:modelValue", data);
};
</script>

<template>
  <FilterModalSection title="size">
    <div class="size-filter">
      <BaseCheckbox
        v-for="(filter, idx) in filters"
        :key="filter.label"
        v-bind="filter"
        @update:model-value="updateValue({ filterIndex: idx, value: $event })"
      />
    </div>
  </FilterModalSection>
</template>

<style lang="scss">
.size-filter {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.5rem;
}
</style>
