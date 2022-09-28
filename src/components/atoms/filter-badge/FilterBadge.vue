<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps<{
  value: Record<string, string | boolean>;
}>();

const emit = defineEmits<{
  (event: "delete", value: string): void;
}>();

const isDisabled = ref(false);

const handleDelete = () => {
  emit("delete", props.value.label as string);
};
</script>

<template>
  <div class="badge">
    <div class="px-5">
      <span
        class="badge__text"
        :class="{ 'badge__text--disabled': isDisabled }"
      >
        {{ value.label }}
      </span>
    </div>

    <div @click="handleDelete" class="badge__remove cursor-pointer">
      <img src="@/assets/images/close.png" alt="close" />
    </div>
    <div @click="isDisabled = !isDisabled" class="badge__action cursor-pointer">
      <img
        :src="require(`@/assets/images/${isDisabled ? 'plus' : 'minus'}.png`)"
        :alt="isDisabled ? 'include' : 'exclude'"
      />
    </div>
  </div>
</template>

<style lang="scss">
.badge {
  font-weight: 400;
  padding: 0.3125rem;
  position: relative;
  text-align: left;
  border-radius: 0.25rem;
  border: 1px solid #d3d3d3;
  color: #343a40;

  &__text {
    line-height: 18px;

    &--disabled {
      text-decoration: line-through;
    }
  }

  &:hover {
    .badge__remove,
    .badge__action {
      display: flex;
    }
  }

  &__remove,
  &__action {
    border-radius: 50%;
    right: -0.5rem;
    font-size: 0.6875rem;
    position: absolute;
    display: none;
    justify-content: center;
    align-items: center;
    background: $primary;
    width: 16px;
    height: 16px;
    padding: 4px;

    img {
      max-width: 100%;
    }
  }

  &__remove {
    top: -0.5rem;
  }

  &__action {
    bottom: -0.5rem;
  }
}
</style>
