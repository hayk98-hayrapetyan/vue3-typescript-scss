<script setup lang="ts">
import BaseCheckbox, {
  ICheckBoxProps,
} from "@/components/atoms/base-checkbox/BaseCheckbox.vue";
import { defineProps, defineEmits } from "vue";
import FilterModalSection from "@/components/molecules/filter-modal-section/FilterModalSection.vue";

export type StageFilterEmit = { filterIndex: number; value: boolean };

defineProps<{
  filters: Array<ICheckBoxProps>;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: StageFilterEmit): void;
}>();

const updateValue = (data: StageFilterEmit) => {
  emit("update:modelValue", data);
};
</script>

<template>
  <FilterModalSection title="stage">
    <div class="stage-filter">
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
.stage-filter {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}
</style>
