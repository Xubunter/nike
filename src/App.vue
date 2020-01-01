<template>
  <div id="app">
    <ProductPage></ProductPage>
  </div>
</template>

<script>
import ProductPage from './components/ProductPage.vue';

export default {
  name: 'app',
  components: {
    ProductPage,
  },
};
</script>

<style lang="scss">
  #app {
    font-family: 'Open Sans', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
</style>
