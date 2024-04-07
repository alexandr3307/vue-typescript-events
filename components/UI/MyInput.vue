<template>
  <input v-model="inputText" type="text" class="input" :placeholder="placeholder" @input="updateParent" />
</template>

<script lang="ts">
  import { defineComponent, ref, PropType } from 'vue';

  export default defineComponent({
    name: 'MyInput',
    props: {
      modelValue: {
        type: [String, Number] as PropType<string | number>,
        default: ''
      },
      placeholder: {
        type: String,
        default: ''
      }
    },
    setup(props, { emit }) {
      const inputText = ref(props.modelValue);

      const updateParent = () => {
        emit('update:modelValue', inputText.value);
      };

      return {
        inputText,
        updateParent
      };
    }
  });
</script>

<style scoped lang="scss">
  .default {
    width: 100%;
  }
  input {
    width: 100%;
    width: -moz-available;
    width: -webkit-fill-available;
    width: fill-available;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 1px solid #C8C8C8;
    border-radius: 8px;
    padding: 8px;
  }
  input:focus {
    background-color: #ffffff;
    outline-width: 0;
  }
</style>
