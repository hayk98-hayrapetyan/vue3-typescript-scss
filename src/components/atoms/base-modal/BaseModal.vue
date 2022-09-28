<template>
  <div class="modal">
    <slot />
  </div>
</template>

<style lang="scss">
.modal {
  height: 100%;
  left: 0;
  outline: 0;
  overflow: hidden;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 1050;
  background: #fff;
}
</style>
