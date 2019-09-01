<template lang="pug">
  label.searchbar
    .searchbar__icon
    input(placeholder="Введите название"
    @input="doAction($event.target.value)").searchbar__input
</template>

<script>
let searchDelay;

export default {
  name: 'catalog-searchbar',
  props: {
    action: Function,
    delay: Number,
  },
  data() {
    return {
      // Выполнить action спустя 500мс
      // Если пользователь ввел что-то сбрасываем отсчет и начинаем снова
      // ++ производительность (меньше запросов к api)
      doAction(value) {
        clearTimeout(searchDelay);
        searchDelay = setTimeout(() => {
          this.action(value);
        }, 500);
      },
    };
  },
};
</script>

<style lang="scss">
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
