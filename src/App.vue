<template lang="pug">
  #app
    .container
      catalog(title="Каталог" :loadLimit=3)
</template>

<script>
import catalog from './components/catalog.vue';

export default {
  name: 'app',
  components: {
    catalog,
  },
  /* data() {
    return {
    movies: [
      {
        title: 'Matrix 4',
        director: 'wtf',
      },
      {
        title: 'John Cena',
        director: 'direktor svejesti',
      },
      {
        title: 'Kowalski',
        director: 'rejiser epta',
      },
      {
        title: 'Kowalski',
        director: 'rejiser epta',
      },
      {
        title: 'Kowalski',
        director: 'rejiser epta',
      },
      {
        title: 'Kowalski',
        director: 'rejiser epta',
      },
    ],
    };
  }, */
};

</script>

<style lang="scss">
@import "scss/main";
</style>
