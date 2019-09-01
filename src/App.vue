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
};

</script>

<style lang="scss">
@import "scss/main";
</style>
