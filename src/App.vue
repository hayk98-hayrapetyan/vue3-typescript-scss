<script setup lang="ts">
import BaseButton from "@/components/atoms/base-button/BaseButton.vue";
import FilterBoard from "@/components/organisms/filter-board/FilterBoard.vue";
import FilterModal from "./components/organisms/filter-modal/FilterModal.vue";

import { reactive, ref } from "vue";
import { countries } from "@/assets/data/coutries";
import { regions } from "@/assets/data/regions";
import { COMPONENT } from "@/enums/filter";

const generateUniqueId = () =>
  new Date().getMilliseconds() + Math.floor(Math.random() * 100);

const filters = reactive([
  {
    title: "Location",
    filters: [
      {
        title: "Country",
        filters: [
          {
            searchable: true,
            component: COMPONENT.CHECKBOX,
            id: generateUniqueId(),
            value: countries.map((country) => ({
              value: country,
              label: country,
              modelValue: false,
            })),
          },
        ],
      },
      {
        title: "Regions",
        filters: [
          {
            searchable: true,
            component: COMPONENT.CHECKBOX,
            id: generateUniqueId(),
            value: regions.map((region) => ({
              value: region,
              label: region,
              modelValue: false,
            })),
          },
        ],
      },
    ],
  },
]);

const renderModal = ref(false);

const toggleFilters = () => {
  renderModal.value = !renderModal.value;
};
</script>

<template>
  <div>
    <div class="app__filters">
      <FilterBoard
        v-for="filter in filters"
        :key="filter.title"
        v-bind="filter"
      />
      <BaseButton @click="toggleFilters" class="px-15 ml-10">
        Advanced filters
      </BaseButton>
    </div>

    <FilterModal v-if="renderModal" @close="toggleFilters" />
  </div>
</template>

<style lang="scss">
.app__filters {
  padding: 40px;
  display: flex;
}
</style>
