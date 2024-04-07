<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
</script>

<style scoped>
  button {
    cursor: pointer;
    border-radius: 5px;
    background: linear-gradient(180deg, rgba(203,163,255,1) 0%, rgba(197,255,176,1) 100%);
    border: navajowhite;
    height: 25px;
  }
</style>
