<script setup lang="ts">
import { BUTTON } from "@/enums/button";
import { withDefaults, defineProps } from "vue";

withDefaults(
  defineProps<{
    type?: BUTTON;
  }>(),
  {
    type: BUTTON.PRIMARY,
  }
);
</script>

<template>
  <button class="btn cursor-pointer" :class="`btn--${type}`">
    <slot />
  </button>
</template>

<style lang="scss">
.btn {
  border: 1px solid;
  border-radius: 2rem;
  display: inline-block;
  font-size: 0.875rem;
  font-weight: 400;
  height: 2.1875rem;
  letter-spacing: 1px;
  padding: 0 1.5625rem;
  text-align: center;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
    border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out,
    -webkit-box-shadow 0.15s ease-in-out;

  &:hover {
    opacity: 1;
  }

  &--primary {
    background-color: $primary;
    border-color: $primary;
    color: #fff;

    &:focus,
    &:hover {
      background-color: transparent;
      border-color: $secondary;
      color: $primary;
    }
  }

  &--secondary {
    background-color: transparent;
    border-color: $secondary;
    color: $primary;

    &:focus,
    &:hover {
      background-color: $primary;
      border-color: $primary;
      color: #fff;
    }
  }

  &--link {
    background-color: transparent;
    border-color: transparent;
    color: $primary;
    padding: 0;
    transition: opacity 0.15s ease-in-out;

    &:focus,
    &:hover {
      outline: none;
      text-decoration: none;
    }
  }
}
</style>
