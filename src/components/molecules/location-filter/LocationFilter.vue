<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import FilterModalSection from "@/components/molecules/filter-modal-section/FilterModalSection.vue";
import BaseSelect, {
  ISelectProps,
} from "@/components/atoms/base-select/BaseSelect.vue";

export type LocationFilterEmit = { filterIndex: number };

defineProps<{
  filters: Array<ISelectProps>;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: LocationFilterEmit): void;
}>();

const updateValue = (filterIndex: number) => {
  emit("update:modelValue", { filterIndex });
};
</script>

<template>
  <FilterModalSection title="LOCATION">
    <BaseSelect :options="filters" @select="updateValue" />
  </FilterModalSection>
</template>
