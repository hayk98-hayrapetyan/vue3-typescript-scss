<script setup lang="ts">
import BaseFilter from "@/components/atoms/base-filter/BaseFilter.vue";
import FilterPopover, {
  IFilterProps,
} from "@/components/molecules/filter-popover/FilterPopover.vue";
import { defineProps, toRefs, ref } from "vue";

export interface IUpdateFilterValue {
  value: boolean;
  optionValue: string;
  sectionIndex: number;
  filterId: string;
}

const props = defineProps<{
  title: string;
  filters: IFilterProps;
}>();

const { filters: localFilters } = toRefs(props);
const activeFilterCount = ref(0);
const showPopover = ref(false);

const updateFilterValue = ({
  sectionIndex,
  filterId,
  value,
  optionValue,
}: IUpdateFilterValue) => {
  const currentFilter = localFilters.value[sectionIndex].filters.find(
    (filter) => filter.id === filterId
  );
  const currentOption = currentFilter.value.find(
    (opt) => opt.value === optionValue
  );
  currentOption.modelValue = value;
  activeFilterCount.value = value
    ? activeFilterCount.value + 1
    : activeFilterCount.value - 1;
};

const hidePopover = () => (showPopover.value = false);
</script>
<script lang="ts">
import clickOutSide from "@mahdikhashan/vue3-click-outside";

export default {
  directives: {
    clickOutSide,
  },
};
</script>

<template>
  <div class="filter-board mx-5" v-click-out-side="hidePopover">
    <BaseFilter :count="activeFilterCount" @click="showPopover = !showPopover">
      {{ title }}
    </BaseFilter>

    <FilterPopover
      v-show="showPopover"
      :filters="localFilters"
      @update:filterValue="updateFilterValue"
    />
  </div>
</template>

<style lang="scss">
.filter-board {
  position: relative;
}
</style>
