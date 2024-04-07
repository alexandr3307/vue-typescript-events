<template>
  <button class="btn" :class="[variant, size]">
    <slot></slot>
  </button>
</template>

<script>
  export default {
    name: "MyButton",
    props: {
      variant: {
        default: 'default',
        type: String,
      },
      size: {
        default: 'md',
        type: String
      }
    }
  };
</script>

<style scoped>
  .btn {
    cursor: pointer;
    display: flex;
    align-items: center;
    font-weight: bold;
    border: none;
  }
  .default {
    color: #ffffff;
    background: #3EA748;
  }
  .md {
    border-radius: 6px;
    width: fit-content;
    padding: 6px 8px;
    gap: 6px;
    height: fit-content;
    font-size: 14px;
    line-height: 20px;
  }
  .lg {
    text-align: center;
    justify-content: center;
    border-radius: 8px;
    font-size: 14px;
    padding: 12px;
  }
  .grey {
    background-color: #EDEDED;
    color: #353535;
  }
  .blue {
    background-color: #1588C9;
    color: #ffffff;
    font-weight: 500;
  }
  .light-blue {
    background-color: #E8F2FE;
    color: #045CC4;
  }
</style>
