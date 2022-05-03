<template>
  <input :value="modelValue" @input="updateInput" class="input" type="text">
</template>

<script>
export default {
  name:'my-input',
  props: {
    modelValue: [String, Number]
  },
  methods: {
    updateInput(event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>
.input {
  width: 100%;
  padding: 10px 15px;
  margin-top: 15px;
  background: #FFFFFF;
  box-shadow: 0px 4px 50px rgba(139, 139, 139, 0.25);
  border-radius: 15px;

  border: none;
  font-family: 'Days One';
  font-style: normal;
  outline:none;
  color: #7E7E7E;
}
</style>