<template lang="pug">
  .catalog
    .catalog__title-wrap
      h1.catalog__title {{title}}
      img(src="../assets/loading-anim.svg" v-if="isBusy").catalog__status
    catalogSearchbar(:action="request").catalog__searchbar
    .catalog__list
      catalog-item(v-for="movie in movies" :key="movie.url" :movie="movie")
      p(v-if="message.length > 0").catalog__message {{ message }}
    .text-center
      catalogButton(text="Показать еще"
        :action="loadMore" v-if="!isEnd").catalog__btn-loadmore
</template>

<script>
/*
  Для теста добавить в template
  p(style="text-align: left; font-size: 11px;" v-for="movie in movies") Movie: {{ movie.title }}
  p(style="text-align: left; font-size: 11px;" v-for="json in jsonData") JSONData: {{ json.title }}
  p(style="text-align: left; font-size: 11px;" v-for="cacheOne in cache") Cache: {{ cacheOne.title }}
*/


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
    catalogItem, catalogSearchbar, catalogButton,
  },
  data() {
    return {
      movies: [],                   // Список фильмов который выводится пользователю
      jsonData: [],                 // То что придет из JSON
      cache: [],                    // Для кеша
      totalCount: 0,                // Общее кол-во фильмов в базе
      isBusy: false,                // Состояние каталога для отображения анимации загрузки
      isEnd: false,                 // Состояние каталога для отображения кнопки загрущить еще
      limitPerLoad: this.loadLimit, // Лимит фильмов для загрузки по кнопке
      message: '',                  // Сообщение для пользователя например об ошибке
    };
  },
  created() {
    this.init();
  },
  methods: {
    init() { // Грузим каталог в первый раз
      const that = this;
      this.isBusy = true; // Анимация загрузки
      axios
        .get('https://swapi.co/api/films/', { timeout: 10000 })
        .catch((error) => {
          this.message = `Ошибка загрузки — ${error}`; // Выводим сообщение в случае ошибки
          this.isBusy = false; // Убираем анимацию загрузки
          this.isEnd = true;
        })
        .then((response) => {
          that.jsonData = response.data.results;

          this.totalCount = response.data.count;

          this.isBusy = false;
          this.isEnd = false;
          this.loadMore();
        });
    },
    searchByQuery(array, query) { // Поиск по текстовому запросу в массиве (например в кеше)
      if (query !== '') {
        return array.filter((elem) => {
          const movieTitle = String(elem.title).toLowerCase();
          return movieTitle.includes(String(query).toLowerCase());
        });
      }
      return array;
    },
    request(query) {
      this.isBusy = true; // Анимация загрузки
      const that = this;

      // 'Поиск по загружнным фильмам'
      // Если кеш не пустой
      // то сделать поиск по нему

      if (this.cache.length > 0) {
        this.movies = this.searchByQuery(this.cache, query);
        this.isBusy = false;
      }

      // 'Поиск новых через api'
      // Если кеш заполнен не всеми фильмами
      // то можно сделать поисковой запрос к api

      if (this.cache.length < this.totalCount) {
        axios
          .get(`https://swapi.co/api/films/?search=${query}`, { timeout: 10000 })
          .catch((error) => {
            this.message = `Ошибка загрузки — ${error}`;
            this.isBusy = false;
            this.isEnd = true;
          })
          .then((response) => {
            that.jsonData = response.data.results;
            this.isBusy = false;
            this.isEnd = false;
            this.movies = [];
            this.loadMore();
          });
      }

      // Если в каталог загружены все фильмы, то удалить кнопку показать еще
      if (this.movies.length === this.totalCount) {
        this.isEnd = true;
      }
    },
    updateCache(data) { // Обновить кеш
      this.cache = Array.from(new Set(this.cache.concat(data))); // Добавляем новые данные
      this.cache = this.cache.reduce((acc, current) => { // Удаляем все дубликаты
        const x = acc.find(item => item.url === current.url);
        if (!x) {
          return acc.concat([current]);
        }
        return acc;
      }, []);
    },
    loadMore() {
      // Кнопка загрузить еще
      // ======================================================
      // Если количество записей в массиве с данными из Json
      // больше или равно лимиту(3) на загрузку, то вевести
      // последние (3) записи из json массива
      // Если меньшье (т.е. записей меньше 3), то вывести
      // все что осталось в Json массиве и удалить
      // кнопку 'Показать еще'
      // ======================================================
      const jsonSize = this.jsonData.length;
      const limit = this.limitPerLoad;

      if (jsonSize >= limit) {
        for (let i = 0; i < limit; i += 1) {
          this.movies.push(this.jsonData.pop());
        }
      } else if (jsonSize < limit) {
        for (let i = 0; i < jsonSize; i += 1) {
          this.movies.push(this.jsonData.pop());
        }
        this.isEnd = true;
      }

      this.updateCache(this.movies);
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

  &__message {
    font-size: 12px;
    color: red;
    padding: 0 10px;
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
