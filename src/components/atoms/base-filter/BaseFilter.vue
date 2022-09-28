<script setup lang="ts">
import { defineProps } from "vue";

defineProps<{
  count: number;
}>();
</script>

<template>
  <div class="filter cursor-pointer">
    <div v-if="count" class="filter__count">{{ count }}</div>
    <slot />
  </div>
</template>

<style lang="scss">
.filter {
  border: 1px solid $secondary;
  border-radius: 2rem;
  padding: 0.3125rem 0.8125rem;
  position: relative;
  height: 35px;
  display: flex;
  align-items: center;

  &__count {
    position: absolute;
    right: -5px;
    top: -7px;
    color: #fff;
    background: #5abf6e;
    border-radius: 50%;
    width: 16px;
    height: 16px;
    font-size: 10px;
    line-height: 11px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>
