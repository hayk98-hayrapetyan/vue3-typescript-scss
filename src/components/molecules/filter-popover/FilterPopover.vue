<script setup lang="ts">
import { IUpdateFilterValue } from "@/components/organisms/filter-board/FilterBoard.vue";
import { defineProps, defineAsyncComponent, defineEmits } from "vue";

export interface IFilters {
  component: string;
  id: string;
  value: string | Record<string, string>;
  searchable: boolean;
}

export interface IFilterProps {
  filters: Array<IFilters>;
}

defineProps<{
  filters: IFilterProps;
}>();

const emit = defineEmits(["update:filterValue"]);

const renderComponent = (componentName: string) =>
  defineAsyncComponent(
    () => import(`@/components/molecules/filters/${componentName}.vue`)
  );

const updateValue = (data: IUpdateFilterValue) => {
  emit("update:filterValue", data);
};
</script>

<template>
  <div class="filter-popover">
    <div v-for="(filter, i) in filters" :key="i">
      <div class="filter-popover__header">
        <span class="filter-popover__header__title pr-15">
          {{ filter.title }}
        </span>
        <img
          src="@/assets/images/filter-green.png"
          alt="filter"
          class="filter-popover__header__icon"
        />
      </div>
      <component
        v-for="{ component, value, id, searchable } in filter.filters"
        :key="id"
        :is="renderComponent(component)"
        :value="value"
        :searchable="searchable"
        :filter-id="id"
        :section-index="i"
        @update:modelValue="updateValue"
      />
    </div>
  </div>
</template>

<style lang="scss">
.filter-popover {
  width: 15.625rem;
  background-color: #fff;
  border-radius: 1rem;
  overflow: hidden;
  border: 1px solid hsla(0, 0%, 44%, 0.12);
  position: absolute;
  left: 0;
  top: calc(100% + 12px);

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.3rem 0.75rem;

    &__title {
      white-space: nowrap;
      display: inline-block;
      font-size: 0.9375rem;
      font-weight: 600;
    }

    &__icon {
      height: 0.75rem;
      width: 0.75rem;
    }
  }
}
</style>
