<template>
    <main class="wrapper">
      <h1>Products</h1>
      <br />
      <div class="card-container">
        <ProductCard v-for="(product, index) in inventory" :key="product.id" :index="index" :product="product" :addToCart="addToCart" :getIcon="getIcon" />
      </div>
    </main>
</template>

<script>
import ProductCard from '@/components/ProductCard-component.vue'
export default {
  props: ['inventory', 'addToCart', 'getIcon', 'showStockMessage'],
  components: {
    ProductCard
  }
}
</script>
