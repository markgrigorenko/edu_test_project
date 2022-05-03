<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button'
}
</script>

<style scoped>
.btn {
  padding: 10px 15px;
  cursor: pointer;
  border: 5px solid #E4E4E4;
  border-radius: 15px;
  font-family: 'Days One';
  font-style: normal;
  text-align: center;
  color: #7E7E7E;
}
</style>