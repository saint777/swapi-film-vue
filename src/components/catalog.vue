<template lang="pug">
  .catalog
    .catalog__title-wrap
      h1.catalog__title {{title}}
      img(src="../assets/loading-anim.svg" v-if="isBusy").catalog__status
    catalogSearchbar.catalog__searchbar
    .catalog__list
      catalog-item(v-for="movie in movies" :movie="movie")
    .text-center
      catalogButton(text="Показать еще" :action="loadMore" v-if="!isEnd").catalog__btn-loadmore
</template>

<script>
import axios from 'axios';
import catalogItem from './catalog-item.vue';
import catalogSearchbar from './catalog-searchbar.vue';
import catalogButton from './catalog-btn.vue';

export default {
  name: 'catalog',
  props: {
    title: String,
    loadLimit: Number,
  },
  components: {
    catalogItem,
    catalogSearchbar,
    catalogButton,
  },
  data() {
    return {
      jsonData: [],
      movies: [],
      isBusy: false,
      isEnd: false,
      limitPerLoad: 3,
    };
  },
  created() {
    this.request();
  },
  methods: {
    request() {
      this.isBusy = true;
      const that = this;
      axios
        .get('https://swapi.co/api/films/')
        .then((response) => {
          that.jsonData = response.data.results;
          this.loadMore();
          this.isBusy = false;
          console.log(this.jsonData);
        });
    },
    loadMore() {
      const jsonSize = this.jsonData.length;
      const limit = this.limitPerLoad;

      if (jsonSize >= limit) {
        for (let i = 0; i < limit; i += 1) {
          this.movies.push(this.jsonData.pop());
        }
      } else if (jsonSize < limit) {
        for (let i = 0; i < jsonSize; i += 1) {
          this.movies.push(this.jsonData.pop());
          this.isEnd = true;
        }
      }
    },
  },
};
</script>

<style lang="scss">
.catalog {
  padding: 22px 30px 30px;

  &__list {
    display: flex;
    flex-wrap: wrap;
    margin-top: 22px;
  }

  &__title-wrap {
    display: flex;
    align-items: center;
  }

  &__title {
    font-family: $fonts;
    font-style: normal;
    font-weight: bold;
    font-size: 24px;
    line-height: 34px;
    color: #222D35;
  }

  &__status {
    width: 30px;
    height: 30px;
    margin-left: 4px;
  }

  &__searchbar {
    margin-top: 18px;
    display: block;
  }

  &__btn-loadmore {
    margin-top: 9px;
  }
}

.text-center {
  text-align: center;
}
</style>
