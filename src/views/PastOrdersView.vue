<template>
   <main class="wrapper">
        <h1>Past Orders</h1>
        <br v-if="!pastOrders.length" />
        <p v-if="!pastOrders.length" class="center"><em>No past orders</em></p>
        <div v-if="displayMessage()" >
          <label style="color: red;font-weight: bold;"> Sorry! There is not enough stock of this product. Take a smaller amount.</label>
          <button class="message-close" @click="closeMessage()">&times;</button>
        </div>
        <br v-if="displayMessage()" />
        <table class="product-table" v-if="pastOrders.length">
            <thead>
                <tr>
                    <td><span class="sr-only">Product Image</span></td>
                    <td>Product</td>
                    <td>Price</td>
                    <td>Quantity</td>
                    <td>Total</td>
                    <td><span class="sr-only">Actions</span></td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(product, i) in pastOrders" :key="i" >
                    <td><i :class="getIcon(product.icon, 'icofont-4x')"/></td>
                    <td>{{ product.name }}</td>
                    <td>${{ product.price.USD.toFixed(2)}}</td>
                    <td>{{ product.quantity }}</td>
                    <td>${{ (product.quantity * product.price.USD).toFixed(2) }}</td>
                    <td><button class="btn btn-dark" @click="addToCart(product.name, product.quantity)">Add</button></td>
                </tr>
            </tbody>
        </table>
    </main>
</template>

<script>
export default {
  props: ['pastOrders', 'getIcon', 'addToCart', 'inventory'],
  methods: {
    closeMessage () {
      this.inventory.forEach(element => {
        element.showStockMessage = false
      })
    },
    displayMessage () {
      const product = this.inventory.find((p) => {
        return p.showStockMessage
      })

      return product
    }
  }
}
</script>
