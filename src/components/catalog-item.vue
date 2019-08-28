<template lang="pug">
  .catalog-item
    .catalog-item__image-wrap
      img.catalog-item__image(src="@/assets/sample.jpg")
    .catalog-item__info
      p.catalog-item__title {{ movie.title }}
      p.catalog-item__director {{ movie.director }}
</template>

<script>
export default {
  name: 'catalogItem',
  props: {
    movie: Object,
  },
};
</script>

<style lang="scss">
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.catalog-item {
  box-shadow: 0px 1px 8px rgba(78, 116, 152, 0.2);
  width: 258px;
  margin-bottom: 22px;
  border-radius: 4px 4px 0px 0px;
  overflow: hidden;
  margin-right: 18px;
  animation: .4s fadeIn;

  &:nth-child(3n) {
    margin-right: 0;
  }

  &__info {
    padding: 12px 11px;
  }

  &__image {
    width: 100%;
  }

  &__image-wrap {
    display: flex;
    align-items: center;
    height: 84px;
    overflow: hidden;
  }

  &__title {
    line-height: 20px;
    color: $bright-blue;
  }

  &__director {
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 16px;
  }
}
</style>
