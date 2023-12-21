<script setup></script>

<template>
  <button>
    <slot></slot>
  </button>
</template>

<style lang="scss" scoped>
button {
  padding: 8px 16px;
  border: none;
  background-color: #0d402a;
  color: white;
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
  cursor: pointer;
}
</style>
