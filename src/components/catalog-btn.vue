<template lang="pug">
  button(@click="action").catalog-btn {{ text }}
</template>

<script>
export default {
  name: 'catalog-btn',
  props: {
    text: String,
    action: Function,
  },
};
</script>

<style lang="scss">
.catalog-btn {
  border: 0;
  font-family: $fonts;
  font-size: 13px;
  line-height: 18px;
  background: $bright-blue;
  border-radius: 4px;
  text-align: center;
  min-width: 190px;
  padding: 9px;
  color: #fff;

  &:hover {
    opacity: 0.9;
  }

  &:active {
    padding: 10px 9px 8px 9px;
  }
}
</style>
