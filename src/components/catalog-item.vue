<template lang="pug">
  .catalog-item
    .catalog-item__image-wrap
      .catalog-item__loader(v-if="isImageLoading")
      img(:src="itemImage").catalog-item__image
    .catalog-item__info
      p.catalog-item__title {{ movie.title }}
      p.catalog-item__director {{ movie.director }}
</template>

<script>
import axios from 'axios';

export default {
  name: 'catalogItem',
  props: {
    movie: Object,
    message: String,
  },
  data() {
    return {
      isImageLoading: false,
      itemImage: '',
    };
  },
  methods: {
    searchImage() { // Поиск картинки по pixabay
      this.isImageLoading = true;
      const that = this;
      axios
        .get(`https://pixabay.com/api/?key=13445987-aabf208eb8d891b48efe137e0&image_type=all&q=${this.movie.title.replace(' ', '+')}`, { timeout: 10000 })
        .catch(() => {
          // В случае ошибки выводим placeholder с сообщение error
          that.itemImage = 'https://via.placeholder.com/258x84.png?text=ERROR';
          that.isImageLoading = false;
        })
        .then((response) => {
          //  data.hits показывает сколько найдено картино
          //  Если найдено - то вывести в нашем item
          //  Если hits==0 то placeholder
          //  отключаем анимацию загрузки
          if (response.data.hits.length > 0) {
            that.itemImage = response.data.hits[0].webformatURL;
          } else {
            that.itemImage = 'https://via.placeholder.com/258x84.png?text=Placeholder';
          }
          that.isImageLoading = false;
        });
    },
  },
  mounted() {
    this.searchImage();
  },
  created() {
    this.searchImage();
  },
};
</script>

<style lang="scss">
.catalog-item {
  box-shadow: 0px 1px 8px rgba(78, 116, 152, 0.2);
  width: 258px;
  margin-bottom: 22px;
  border-radius: 4px 4px 0px 0px;
  overflow: hidden;
  margin-right: 18px;

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
    position: relative;
  }

  &__loader {
    width: 100%;
    height: 100%;
    top: 12px;
    position: absolute;
    background-image: url('./../assets/loading-anim.svg');
    background-size: 50px;
    background-position: center;
    background-repeat: no-repeat;
    background-color: #fff;
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
