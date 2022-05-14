<template>
    <div class="card" v-if="product.stockQuantity > 0">
        <div class="card-title">
        {{product.name}}
        </div>
        <div class="card-body">
        <i :class="getIcon(product.icon, 'icofont-10x')"></i>
        <form>
            <div class="row">
            <div class="cell">
                <label>Type:</label>
            </div>
            <div class="cell">
                <em>{{product.type}}</em>
            </div>
            </div>
            <div class="row">
            <div class="cell">
                <label>Price:</label>
            </div>
            <div class="cell">
                ${{product.price.USD.toFixed(2)}}
            </div>
            </div>
            <div class="row">
            <div class="cell">
                <label>Stock Quantity:</label>
            </div>
            <div class="cell">
                {{product.stockQuantity}}
            </div>
            </div>
            <div class="row">
            <div class="cell">
                <label>Quantity:</label>
            </div>
            <div class="cell">
                <input type="number" v-model.number="quantity">
            </div>
            </div>
            <div class="row" v-if="product.showStockMessage">
            <div>
                <br />
                <label style="color: red;font-weight: bold;">Sorry! There is not enough stock of this product. Take a smaller amount.</label>
            </div>
            </div>
        </form>
        </div>
        <div class="card-footer">
        <button @click="addToCart(product.name, quantity); changeQuantity()" class="btn btn-light">
            Add to cart
        </button>
        </div>
    </div>
</template>

<script>
export default {
  props: ['product', 'index', 'addToCart', 'getIcon'],
  data () {
    return {
      quantity: 0
    }
  },
  methods: {
    changeQuantity () {
      this.quantity = 0
    }
  }
}
</script>
