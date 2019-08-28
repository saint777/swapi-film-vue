<template lang="pug">
  label.searchbar
    input(placeholder="Введите название").searchbar__input
    catalogSearchIcon.searchbar__icon
</template>

<script>
import catalogSearchIcon from '../assets/search-icon.svg';

export default {
  name: 'catalog-searchbar',
  components: {
    catalogSearchIcon,
  },
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
    position: absolute;
    left: 12px;
    top: 50%;
    transform: translateY(-50%);
  }
}
</style>
