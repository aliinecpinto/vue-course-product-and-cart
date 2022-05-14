<template>
  <div class="home">

    <div class="splash-container">
      <div class="splash">
        <h1>Splendid Food</h1>
      </div>
    </div>

    <main class="wrapper">

      <h2>Recommended</h2>

      <div class="recommended">
        <ProductCard v-for="(product, index) in recommended" :key="product.id" :index="index" :product="product" :addToCart="addToCart" :getIcon="getIcon"/>
      </div>

    </main>
  </div>
</template>

<script>
import ProductCard from '@/components/ProductCard-component.vue'
export default {
  name: 'HomeView',
  props: ['recommended', 'addToCart', 'getIcon', 'showStockMessage'],
  components: {
    ProductCard
  }
}
</script>
