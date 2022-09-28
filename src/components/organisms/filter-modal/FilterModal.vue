<script setup lang="ts">
import BaseModal from "@/components/atoms/base-modal/BaseModal.vue";
import BaseButton from "@/components/atoms/base-button/BaseButton.vue";
import SizeFilter from "@/components/molecules/size-filter/SizeFilter.vue";
import StageFilter from "@/components/molecules/stage-filter/StageFilter.vue";
import LocationFilter from "@/components/molecules/location-filter/LocationFilter.vue";
import FoundedFilter from "@/components/molecules/founded-filter/FoundedFilter.vue";
import FilterBadge from "@/components/atoms/filter-badge/FilterBadge.vue";

import { BUTTON } from "@/enums/button";
import { ref, reactive, computed, defineEmits } from "vue";
import { countries } from "@/assets/data/coutries";

const emit = defineEmits<{
  (event: "close"): void;
}>();

const tabs = ref([
  "Organisation Info",
  "Activity sectors",
  "Impact metrics",
  "Funding rounds",
  "Patents",
  "Wildcard match",
  "Custom filters",
]);

const locationFilter = reactive({
  filters: countries.map((country) => ({
    value: country,
    label: country,
    modelValue: false,
  })),
  setValue: ({ filterIndex }) => {
    locationFilter.filters[filterIndex].modelValue = true;
  },
  deleteValue: (label) => {
    const currentFilter = locationFilter.filters.find(
      (opt) => opt.label === label
    );
    currentFilter.modelValue = false;
  },
  selectedValue: computed(() => {
    return locationFilter.filters.filter((opt) => {
      console.log("called", opt.modelValue);
      return opt.modelValue;
    });
  }),
});

const foundedYearFilter = reactive({
  filters: {
    start: "",
    end: "",
  },
  setValue: ({ start, end }: Record<string, string>) => {
    const val = start || end;
    foundedYearFilter.filters[start ? "start" : "end"] = val;
  },
  deleteValue: () => {
    locationFilter.filters = {
      start: "",
      end: "",
    };
  },
  selectedValue: computed(() => {
    const { start, end } = foundedYearFilter.filters;

    if (!start && !end) return [];

    let value;
    if (!!start && !end) {
      value = `From ${start}`;
    } else if (!start && !!end) {
      value = `To ${end}`;
    } else {
      value = `${start} - ${end}`;
    }

    return [
      {
        value: value,
        label: value,
      },
    ];
  }),
});

const sizeFilter = reactive({
  filters: [
    { label: "1-10", value: "1-10", modelValue: false },
    { label: "101 - 200", value: "101 - 200", modelValue: false },
    { label: "1001 - 5000", value: "1001 - 5000", modelValue: false },
    { label: "11 - 50", value: "11 - 50", modelValue: false },
    { label: "201 - 500", value: "201 - 500", modelValue: false },
    { label: "5001 - 10000", value: "5001 - 10000", modelValue: false },
    { label: "51 - 100", value: "51 - 100", modelValue: false },
    { label: "501 - 1000", value: "501 - 1000", modelValue: false },
    { label: "10001+", value: "10001+", modelValue: false },
  ],
  setValue: ({ filterIndex, value }) => {
    sizeFilter.filters[filterIndex].modelValue = value;
  },
  deleteValue: (label) => {
    const currentFilter = sizeFilter.filters.find((opt) => opt.label === label);
    currentFilter!.modelValue = false;
  },
  selectedValue: computed(() =>
    sizeFilter.filters.filter((opt) => opt.modelValue)
  ),
});

const stageFilter = reactive({
  filters: [
    { label: "Ideation", value: "Ideation", modelValue: false },
    { label: "Early", value: "Early", modelValue: false },
    { label: "Growth", value: "Growth", modelValue: false },
    { label: "Scaling", value: "Scaling", modelValue: false },
  ],
  deleteValue: (label) => {
    const currentFilter = stageFilter.filters.find(
      (opt) => opt.label === label
    );
    currentFilter!.modelValue = false;
  },
  setValue: ({ filterIndex, value }) => {
    stageFilter.filters[filterIndex].modelValue = value;
  },
  selectedValue: computed(() =>
    stageFilter.filters.filter((opt) => opt.modelValue)
  ),
});

const closeModal = () => {
  emit("close");
};
</script>

<template>
  <BaseModal class="filter-modal">
    <div class="filter-modal__tabs px-15 pt-25">
      <ul>
        <li
          v-for="tab in tabs"
          :key="tab"
          class="filter-modal__tabs__tab my-15 mx-5 cursor-pointer"
        >
          {{ tab }}
        </li>
      </ul>
    </div>

    <div class="filter-modal__board">
      <div class="filter-modal__board__filters custom-scrollbar p-30 m-10">
        <LocationFilter
          :filters="locationFilter.filters"
          @update:model-value="locationFilter.setValue"
        />
        <FoundedFilter
          :filters="foundedYearFilter.filters"
          @update:model-value="foundedYearFilter.setValue"
        />
        <SizeFilter
          :filters="sizeFilter.filters"
          @update:model-value="sizeFilter.setValue"
        />
        <StageFilter
          :filters="stageFilter.filters"
          @update:model-value="stageFilter.setValue"
        />
      </div>

      <div>
        <div class="filter-modal__board__results py-15 px-30">
          <FilterBadge
            v-for="filter in locationFilter.selectedValue"
            :value="filter"
            :key="filter.label"
            @delete="locationFilter.deleteValue"
            class="filter-modal__board__results__item my-5"
          />
          <FilterBadge
            v-for="filter in foundedYearFilter.selectedValue"
            :value="filter"
            :key="filter.label"
            @delete="foundedYearFilter.deleteValue"
            class="filter-modal__board__results__item my-5"
          />
          <FilterBadge
            v-for="filter in sizeFilter.selectedValue"
            :value="filter"
            :key="filter.label"
            @delete="sizeFilter.deleteValue"
            class="filter-modal__board__results__item my-5"
          />
          <FilterBadge
            v-for="filter in stageFilter.selectedValue"
            :value="filter"
            :key="filter.label"
            @delete="stageFilter.deleteValue"
            class="filter-modal__board__results__item my-5"
          />
        </div>

        <div class="filter-modal__board__footer py-10 px-30">
          <span>44,777 results</span>
          <div class="filter-modal__board__footer__actions">
            <BaseButton class="filter-modal__board__footer__actions__item">
              APPLY
            </BaseButton>
            <BaseButton
              class="filter-modal__board__footer__actions__item"
              :type="BUTTON.SECONDARY"
            >
              CLEAR ALL
            </BaseButton>
            <BaseButton @click="closeModal" :type="BUTTON.LINK">
              CANCEL
            </BaseButton>
          </div>
        </div>
      </div>
    </div>
  </BaseModal>
</template>

<style lang="scss">
.filter-modal {
  display: flex;

  &__tabs {
    max-width: 16.6666666667%;
    width: 100%;
    border-right: 1px solid hsla(0, 0%, 44%, 0.12);
    height: 100%;
    background-color: #f8fcfc;

    &__tab {
      text-transform: uppercase;
      list-style: none;
      line-height: 1.5rem;
      display: inline-block;
      width: 100%;
      font-size: 0.875rem;

      &:first-child {
        color: $primary;
        font-weight: 600;
      }
    }
  }

  &__board {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: space-between;
    flex-direction: column;

    &__results {
      display: flex;
      flex-wrap: wrap;
      border-top: 1px solid hsla(0, 0%, 44%, 0.12);

      &__item {
        margin-right: 0.5rem;
      }
    }

    &__footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      border-top: 1px solid hsla(0, 0%, 44%, 0.12);

      &__actions {
        display: flex;

        &__item {
          margin-right: 0.5rem;
        }
      }
    }
  }
}
</style>
