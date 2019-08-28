<template lang="pug">
  label.searchbar
    .searchbar__icon
    input(placeholder="Введите название").searchbar__input
</template>

<script>

export default {
  name: 'catalog-searchbar',
};
</script>

<style lang="scss" scoped>
.searchbar {
  display: block;
  position: relative;

  &__input {
    width: 100%;
    padding: 8px 39px;
    border: 1px solid #E4E4E4;
    border-radius: 4px;
    font-family: $fonts;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    line-height: 20px;
    outline: none;
    color: #536069;

    &:focus {
      border: 1px solid $bright-blue;
    }

    &:focus::placeholder {
      color: rgba(0,0,0,0);
    }

    &::placeholder {
      color: #C3D0D8;
    }
  }

  &__icon {
    width: 15px;
    height: 16px;
    background-image: url('./../assets/search-icon.svg');
    background-repeat: no-repeat;
    position: absolute;
    left: 12px;
    top: 50%;
    transform: translateY(-50%);
  }
}
</style>
