<script setup lang="ts">
import { defineProps } from "vue";

defineProps<{ title: string }>();
</script>

<template>
  <div class="section mt-30">
    <div class="section__title">{{ title }}</div>

    <slot />
  </div>
</template>

<style lang="scss">
.section {
  &__title {
    line-height: 1.5rem;
    font-size: 0.875rem;
    text-transform: uppercase;
    margin-bottom: 0.625rem;
    display: flex;

    &:after {
      content: "";
      border-bottom: 1px solid hsla(0, 0%, 44%, 0.12);
      flex: 1 1;
      margin: auto auto auto 0.5rem;
    }
  }
}
</style>
