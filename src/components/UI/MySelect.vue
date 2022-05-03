<template>
  <select class="selectStyle" v-model="modelValue" @change="changeOption">
    <option class="selectStyle" disabled value="">Отсортировать...</option>
    <option class="selectStyle"
        v-for="option in options"
        :key="option.value"
        :value="option.value">
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'my-select',
  props: {
    modelValue: {
      type: String
      },
    options: {
      type: Array,
      default: () => []
      }
    },
    methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value);
    }
  }
}
</script>

<style scoped>
.selectStyle {
  outline: none;
  padding: 10px 15px;
  border: 5px solid #E4E4E4;
  border-radius: 15px;
  font-family: 'Days One';
  font-style: normal;
  border: 5px solid #E4E4E4;
  border-radius: 15px;
  font-family: 'Days One';
  cursor: pointer;
  text-align: center;
  color: #7E7E7E;
}
</style>