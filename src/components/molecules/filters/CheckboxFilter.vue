<script setup lang="ts">
import BaseCheckbox, {
  ICheckBoxProps,
} from "@/components/atoms/base-checkbox/BaseCheckbox.vue";
import { IUpdateFilterValue } from "@/components/organisms/filter-board/FilterBoard.vue";
import { defineProps, defineEmits, computed, ref } from "vue";
import SearchFilter from "./SearchFilter.vue";

const props = defineProps<{
  value: Array<ICheckBoxProps>;
  searchable: boolean;
  sectionIndex: number;
  filterId: string;
}>();

const emit = defineEmits<{
  (event: "update:modelValue", value: IUpdateFilterValue): void;
}>();

const searchText = ref("");

const options = computed(() => {
  return props.value.filter((opt) =>
    opt.label.toLowerCase().includes(searchText.value.toLowerCase())
  );
});

const updateValue = (value: boolean, optionValue: string) => {
  console.log(value, "val", optionValue);

  emit("update:modelValue", {
    value,
    optionValue,
    sectionIndex: props.sectionIndex,
    filterId: props.filterId,
  });

  console.log(searchText.value, "searchText.value");
  searchText.value = "";
  console.log(searchText.value, "searchText.value");
};
</script>

<template>
  <div>
    <SearchFilter v-if="searchable" v-model="searchText" />
    <ul class="checkbox-filter custom-scrollbar">
      <li
        v-for="option in options"
        :key="option.label"
        class="checkbox-filter__list"
      >
        <BaseCheckbox
          v-bind="option"
          @update:modelValue="updateValue($event, option.value)"
        />
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
.checkbox-filter {
  list-style: none;
  max-height: 184px;

  &__list {
    border-radius: 0 !important;
    color: #232426;
    margin: 0 !important;
    overflow: hidden;
    padding: 0.4rem 0.75rem;
    text-overflow: ellipsis;
    white-space: nowrap;
    width: 100%;

    &:hover {
      background-color: #c4ffcf;
    }
  }
}
</style>
